	- put it next to `DTO` directory.
```csharp
dotnet add package AutoMapper
```

Add service.
```csharp
builder.Services.AddAutoMapper(cfg => {
}, AppDomain.CurrentDomain.GetAssemblies());
```

---

```csharp
public class ProfileAddressMapper : Profile
{
	public ProfileAddressMapper()
	{
		// for complex mapping.
		CreateMap<User, ProfileAddressDto>()
		.ForMember(dest => dest.Country, opt => opt.MapFrom(src => src.Address.Country))
		.ForMember(dest => dest.City, opt => opt.MapFrom(src => src.Address.City))
		.ForMember(dest => dest.Description, opt => opt.MapFrom(src => src.Address.Description))
		.ForMember(dest => dest.ZipCode, opt => opt.MapFrom(src => src.Address.ZipCode));
		
		// If the source has null value, then don't take it.
		// src  => The source object	    { Street: "123 Main St", City: "Boston" }
		// dest => The destination object 	{ Address: null, Name: "John" }
		// srcMember => The actual value of the source property being mapped  ->  "123 Main St" or null
		// in .ForAllMembers its best not to use src only, because you can use it for one-to-one interaction with specific property.
		CreateMap<ProfileAddressDto, User>()
			.ForAllMembers(opts => opts.Condition((src, dest, srcMember) => srcMember != null));

		CreateMap<ProfileAddressDto, Address>()
			.ForAllMembers(opts => opts.Condition((src, dest, srcMember) => srcMember != null));
	}
}
```


Ignore null values keep them as they are.
```csharp

 
CreateMap<ProfileAddressDto, User>()
    .ForAllMembers(opts => opts.Condition((src, dest, srcMember) => srcMember != null));

CreateMap<ProfileAddressDto, Address>()
    .ForAllMembers(opts => opts.Condition((src, dest, srcMember) => srcMember != null));
```

More complex
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
```