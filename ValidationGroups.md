# 🏷️ Validation Groups

## ❓ What are Groups?

- Groups are used to `check only some fields` in `certain situations`.  
   - Example: Enrollment, ExamRegistration.  
- You assign a field to a group using the `groups` attribute in the annotation.

---

## 🟢 Default Group & @Validated Behavior

- Fields without groups belong to the default group.  
- Using `@Valid` only validates the default group.  
   - Default group fields are checked only if you don’t specify any group.  
   - If you don’t specify any group, only default group fields are validated; fields in other groups are ignored.  

- Using `@Validated`  
   - When you check a specific group by using `@Validated` → default group fields are ignored.

---

## 3️⃣ Custom Groups

- Create a custom interface as a marker for the group:
  ``` java
     public interface EnrollGroup {}

    // Use in Annotation
    @NotNull(groups = EnrollGroup.class)
    private String parentContact;

    @NotEmpty
    private String phoneNumber;

    // This field is validated only when we use @validate(EnrollGroup.class) in Controller layer and validating phoneNumber is ignored
  ```

## ⚙️ How to Validate Groups

- **Using `@Validated` in Controller**  
- Fields with a custom group are validated only when that group is used.

---

## 📋 Bean Validation: Groups & Annotations

### 1️⃣ Multiple Annotations, Different Groups

- You can assign multiple groups to a different annotation:

  ``` java
  
    public interface EnrollGroup {}
  
   @NotNull(groups = Default.class)
   @NotEmpty(groups= EnrollGroup.class)
   private String parentContact;

   @NotEmpty
   private String phoneNumber

   // if we use @Valid then the parentContact  will  be validated with only @NotNull and phoneNumber will be validated with @NotEmpty using Default.class 
   // if we use @Validated then parentContact will  be validated using EnrollGroup.class and phoneNumber will be ignored

   
  ```

- **Validation behavior:**

  | Trigger | What gets validated? |
  |---------|--------------------|
  | `@Valid` | `@NotNull` (Default group) only |
  | `@Validated(EnrollGroup.class)` | `@NotEmpty` (EnrollGroup) only |

---

### 2️⃣ Single Annotation, Multiple Groups

 - You can assign multiple groups to a single annotation:

  ``` java
   public interface EnrollGroup {}
  
   @NotNull(groups = {Default.class, EnrollGroup.class})
   private String parentContact;

   @NotEmpty
   private String phoneNumber

   // if we use @Valid then the parentContact and phoneNumber will  be validated using Default.class 
   // if we use @Validated then parentContact will  be validated using EnrollGroup.class and phoneNumber will be ignored

   // whenever we are using multiple groups including Default.class which is Default group then 
    the field is valid either if we use @Valid or @Validated in Controller Layer
  ```

**Validation behavior:**

  | Trigger | What gets validated? |
  |---------|--------------------|
  | `@Valid` | Validates because Default group is included |
  | `@Validated(EnrollGroup.class)` | Validates because EnrollGroup is included |

---

## 💡 Thumb Rules

- Default group → validated by `@Valid` or when no group specified.  
- Custom group → validated only when explicitly triggered (`@Validated(CustomGroup.class)` or Validator).  
- Multiple annotations → each works independently with its group.  
- Single annotation with multiple groups → validates for any triggered group.
