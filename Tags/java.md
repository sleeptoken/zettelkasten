

```java
public class Cat extends Animal{
	@Override
	public void eat(){
	    ...
	}
}
```
Above code is overriding a method in the animal class with what we are defining here in the cat class 
- If we alter the method signature by changing the parameters that the method takes, the code will no longer compile
- The annotation of `@Override` will lead in to a compile error

> so remove the `@Override` annotation and then change the method signature 


