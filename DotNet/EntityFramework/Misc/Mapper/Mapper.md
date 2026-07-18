### Setup
1. put it next to `DTO` directory.
```csharp
dotnet add package AutoMapper
```
2. Add service.
```csharp
builder.Services.AddAutoMapper(cfg => {}, AppDomain.CurrentDomain.GetAssemblies());
```

---
### Notes
- If you have childDto and parentDto "Like Address And User". if you defind the childDto mapping, then automapper will automatically map it for you in parentDto.


---
### Templates
```csharp
public class ProfileAddressMapper : Profile
{
	public ProfileAddressMapper()
	{
		CreateMap<User, ProfileAddressDto>()
			.ForMember(dest => dest.Id, opt => opt.MapFrom(_ => Guid.NewGuid())) // Setting new guid.
			
			// if CreateMap<ChildClass, ChildClassDto>  you can do ->
			.ForMember(dest => dest.ChildClassDto, opt => opt.MapFrom(src => src.ChildClass));
	}
}
```

---

### Ignore null values
- Note that `bool?` is actually an object that has property that may point at null. so `srcMember != null` alone will fail
```csharp
// 1. This work in update dto, but it will do unwanted includes if you used it in Projection<>
// 2. Don't use this in output dtos, use it only with update dtos.
.ForAllMembers(opts => opts.Condition((src, dest, srcMember) => 
	src.GetType().GetProperty(opts.DestinationMember.Name)?.GetValue(src) != null));

// this may fail if you have bool? or int?
CreateMap<ProfileAddressDto, User>()
    .ForAllMembers(opts => opts.Condition((src, dest, srcMember) => srcMember != null)); 
```


### Projection
- In complex dtos, we will fail to use naive mapper with projection.
- Put it in `Dto` which is better than putting it in `mapper`.
```csharp
var product = await _context.Product
        .AsNoTracking()
        .Where(p => p.Id == productId)
        .ProjectTo<UserDto>(_mapper.ConfigurationProvider) // EF Core translates this to SQL
        .FirstOrDefaultAsync();
```

---
### AfterMap

> [!warning]
> If you used AfterMap, it will break your logic when use `.ProjectTo()`. Try as hard you can to keep using `ForMember()`. If you can't, do mapping using constructor of the classes.

```csharp
CreateMap<ProductProfileDto, Product>()
    .ForMember(dest => dest.Name, opt => opt.MapFrom(src => src.ProductName))
    // We ignore Variants during the automatic phase to handle it manually
    .ForMember(dest => dest.Variants, opt => opt.Ignore()) 
    .AfterMap((src, dest, context) => 
    {
        var combinedVariants = new List<ProductVariant>();

        // 1. Add Colors
        if (src.ColorVariants != null)
        {
            var colors = context.Map<List<ProductVariant>>(src.ColorVariants);
            combinedVariants.AddRange(colors);
        }

        // 2. Add Sizes (Assuming you want to create a variant record for each size)
        if (src.SizeVariants != null)
        {
            foreach (var size in src.SizeVariants)
            {
                combinedVariants.Add(new ProductVariant { Size = size });
            }
        }

        dest.Variants = combinedVariants;
    });
```


---
## Usage


> [!Danger]
> You are doing DI to `IMapper _mapper` not for the Mapper we have created.


```csharp    
public UsersController(IMapper mapper)
{
    _mapper = mapper;
}

// Create new target Object.
_mapper.Map<ProductDto,Product>(dto);
// or Simpler and faster put the target only let compiler decide the sorece type
_mapper.Map<Product>(dto);

// Copy value from object to other one.
_mapper.Map(dto, existingProduct);

// In Custom Select statment 
var productDto = await _context.Product
        .AsNoTracking()
        .Where(p => p.Id == productId)
        .ProjectTo<ProductProfileDto>(_mapper.ConfigurationProvider) // Magic happens here
        .FirstOrDefaultAsync();
```