- It can determine which numbers are prime or not in **O(N loglogn)**
- Which iterate throw every prime number and computes its factors  
```
2*3 2*4 2*5 2*6 2*7
3*3 3*4 3*5 3*6 3*7   
```
- so our iteration will be as would be N and everytime it will shrink as follows:
$$
N +(\frac{N}{2} + \frac{N}{3} + \frac{N}{5}+\frac{N}{7}+...\frac{N}{LastPrime})
$$
```java
public static boolean[] sieve(int N){  
  
    boolean [] tempArr= new int[N];  
    for(int i =2 ; i <= N ; i++)  
        tempArr[i] = true;  
    for(int i = 4 ; i <= N ; i+=2) // All evens are not prime  
        tempArr[i]= false;  
  
    for(int i=3 ; i <= N ; i+=2) // Iterate over odd numbers only  
        if(tempArr[i])  
            for(int j = 2* i ; j <= N ; j += i)  
                tempArr[j] = false;  
    return tempArr;  
}
```