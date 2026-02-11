# 📘 Custom Validation Annotations

- Custom annotations are user-defined validation rules used when built-in annotations are not sufficient.  
- They are created using:
   - 📝 **Custom annotation interface**  
   - ⚙️ **ConstraintValidator implementation**

---

## 🛠️ Steps to Create a Custom Annotation

### 1️⃣ Create the Annotation Interface

Must include:  

- `@Constraint(validatedBy = YourValidatorClass.class)` → points to validator  
- `@Target(...)` → where the annotation can be applied (field, method, etc.)  
- `@Retention(RetentionPolicy.RUNTIME)` → available at runtime  
- `message, groups, payload` → standard attributes

``` java

    import jakarta.validation.Constraint;
    import jakarta.validation.Payload;
    import java.lang.annotation.*;

    @Documented
    @Constraint(validatedBy = PasswordValidator.class)
    @Target({ ElementType.FIELD, ElementType.METHOD })
    @Retention(RetentionPolicy.RUNTIME)
    public @interface ValidPassword {
        String message() default "Password is invalid";
        Class<?>[] groups() default {};
        Class<? extends Payload>[] payload() default {};
    }
    

```

---

### 2️⃣ Implement the Validator

- Implements `ConstraintValidator<AnnotationType, FieldType>`  
- Override the `isValid()` method to define your validation logic

``` java

    import jakarta.validation.ConstraintValidator;
    import jakarta.validation.ConstraintValidatorContext;

    public class PasswordValidator implements ConstraintValidator<ValidPassword, String> {
    
        @Override
        public boolean isValid(String password, ConstraintValidatorContext context) {
            if (password == null) return false;
            // Must contain at least 1 uppercase, 1 digit, 1 special char
            return password.matches("^(?=.*[A-Z])(?=.*\\d)(?=.*[@#$%^&+=!]).+$");
        }
    }

```

---

### 3️⃣ Use the Custom Annotation in DTO

- Apply the annotation to your DTO fields

---

### 4️⃣ Use in Controller

- Spring Boot automatically invokes your custom validator and throws `MethodArgumentNotValidException` if validation fails.

---

## 🎯 @Target – Where Annotation Can Be Applied

| Target Type | Use in Single Line |
|------------|------------------|
| `ElementType.FIELD` | Apply annotation on class fields |
| `ElementType.METHOD` | Apply annotation on methods (getter/setter or other methods) |
| `ElementType.PARAMETER` | Apply annotation on method parameters |
| `ElementType.TYPE` | Apply annotation on class, interface, or enum |
| `ElementType.ANNOTATION_TYPE` | Apply annotation on another annotation (meta-annotation) |
| `ElementType.CONSTRUCTOR` | Apply annotation on constructors |
| `ElementType.LOCAL_VARIABLE` | Apply annotation on local variables inside methods |
| `ElementType.PACKAGE` | Apply annotation on a package |
| `ElementType.TYPE_USE` | Apply annotation wherever a type is used (e.g., generics) |

✅ **Tip:** Most custom validations use `FIELD` or `METHOD`.

---

## 📌 @Retention – How Long Annotation is Retained

| Retention Type | Use in Single Line |
|----------------|------------------|
| `RetentionPolicy.SOURCE` | Only in source code, discarded by compiler |
| `RetentionPolicy.CLASS` | Stored in `.class` file, not available at runtime |
| `RetentionPolicy.RUNTIME` | Available at runtime for reflection/validation |

✅ **Tip:** For Spring Boot validation, always use `RUNTIME` so validator can read annotation at runtime.
