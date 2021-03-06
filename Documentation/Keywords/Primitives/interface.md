# Keywords/Primitives
## `interface`

An interface is a type of [primitive](Primitives.md) that is intended to be used as a template for deriving [classes](class.md) and [structures](struct.md). Because of this, its characteristics are compatible for both types.
- Nullable.
- Can only derive from another interface.
- Other classes, structures, or interfaces can derive from an interface.
- Cannot be marked abstract.
- Cannot be marked static.
- Cannot contain an entry method.
- Cannot contain a constructor.
- Can assign fields in their declarations.
- A non-type object included does not require a body.

Interfaces have the ability to not declare a body for a field, method, or other non-type object (excluding constructors). When a class or structure derives from the interface, it will have to implement the body for any non-type objects without a body. Interfaces *can* be null, but a deriving structure never will be when cast back to its interface form. However, a null class casted to a null interface will be null. Interfaces also cannot use the [`abstract`](../Implementation/abstract.md) keyword, since by default an interface already has similar behavior.

Interfaces deriving from other interfaces do not have to implement bodies, however a class or structure deriving from it will have to implement bodies from as far back as necessary.

An interface can choose to declare a body for its non-type objects, and in that case a class or structure deriving from it is not required to implement its own. However, it still can by using the [`override`](../Implementation/override.md) keyword. After being overridden, a non-type object can have things such as its return type changed.

```nsharp
public namespace Example
{
    public interface TypeA
    {
        public int FieldA;
        public int FieldB;
        public int FieldC = 5;
        // `FieldC` cannot be assigned to here.

        public void MethodA(); // No-body method.
        public void MethodB() { } // Bodied method.

        public interface TypeB { } // This is created here, but will not need to be implemented.
    }

    public interface TypeC : TypeA
    {
        // Other interfaces do not need to implement other non-type objects.

        public int FieldD;
        
        public void MethodC() { }
    }

    public class TypeD : TypeC
    {
        public int FieldA = 1; // Now that we are in a class, we can initialize here.
        // Missing `FieldB`. This will throw a compiler error.
        public int FieldC;
        public int FieldD;

        public TypeD()
        {
            FieldA = -1;
            FieldB = -1;
            FieldC = -1;
            FieldD = -1;

            MethodB(); // Non-implemented methods with bodies can still be called.
            MethodC();

            base.MethodC(); // To go up a parent, use `base`.
            // This is calling the non-overriden `MethodC()`.
        }

        public override void MethodC() { } // Overridden body from `TypeC.MethodC()`
    }
}
```
