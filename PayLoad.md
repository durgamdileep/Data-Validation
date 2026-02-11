# 📦 Purpose of Payload

- Used in validation annotations to `attach metadata or categorize errors`.
- Optional in most projects; mainly for `advanced error handling`.
- The `payload attribute is an array`, so even if you have one class, you `must wrap it in { }`.

---

## 🔑 Key Rule for Payload

- Must be a class implementing `jakarta.validation.Payload`.  
- `Interfaces alone cannot be used in payload`.  
- Inner/nested classes must be static if inside another class.  
- You can choose your own names for interfaces, classes, and nested classes — there are no default names.
- If the `class is nested/inner inside another class or interface`, you `must use the parent name` (e.g., ErrorLevel.Critical.class).
- If it is a `top-level class` (not nested), you can `use it directly` (e.g., Critical.class).

---

## ✅ Correct Ways to Create Payload

### 🅰️ Option A: Simple class
  ``` java
     import jakarta.validation.Payload;

     public class Critical implements Payload {}
     public class Warning implements Payload {}

     // Usage
     @NotNull(payload= {Critical.class})
     private String email;

     @NotEmpty(payload ={Warning.class})
     private String username;

  ```

### 🅱️ Option B: Class implementing marker interface
  ``` java

    public interface ErrorLevel {} // optional marker interface

    public class Critical implements Payload, ErrorLevel {}
    public class Warning implements Payload, ErrorLevel {}

    // Usage

    @NotNull(payload= {Critical.class})
    private String email;

    @NotEmpty(payload ={Warning.class})
    private String username;

    @NotBlank(payload= {Critical.class})
    private String phoneNumber;

    
    
  ```

### 🅾️ Option C: Nested class inside interface (grouping) **(Recommended)**
  ``` java

    public interface ErrorLevel { // optional marker interface

           public class Critical implements Payload {}  // implicitly public and static
           public class Warning implements Payload {}   // implicitly public and static
    }  
    

   // Usage
   @NotNull(payload = {ErrorLevel.Critical.class})
   private String email;

   @NotBlank(payload= {ErrorLevel.Warning.class})
    private String phoneNumber;

   // Interface members
      1. Nested classes → implicitly public and static.
      2. Variables (fields) → implicitly public, static, and final.
      3. Methods → implicitly public and abstract, not static (unless you explicitly declare static or default).

  
  ```

### 🅳 Option D: Nested class inside class (grouping)
  ``` java

     public class ErrorLevel {

      // Nested class must be static
      public static class Critical implements Payload {}
      public static class Warning implements Payload {}
   }


   // Usage
    @NotNull(payload = {ErrorLevel.Critical.class})
    private String email;

    @NotBlank(payload= {ErrorLevel.Warning.class})
    private String phoneNumber;

  ```

---

## 🌟 Why Option C is Recommended

- No need for **static** keyword — classes inside interfaces are implicitly static.  
- Clean organization — groups related payload classes (e.g., Critical, Warning) under a single interface.  
- Optional marker interface — makes your code easy to maintain and read.  
- Simpler usage in annotations — just `ErrorLevel.Critical.class`

---

## ⚠️ Incorrect Ways

### Using only an interface
  ``` java
    public interface Critical extends Payload {}

    // Usage
    @NotNull(payload = {Critical.class}) // ❌ Does NOT work
    private String email;
    
  ```
- Reason: `payload requires a concrete class`, not an interface.

### Class not implementing Payload
  ``` java
    public class Critical {}

   // Usage
     @NotNull(payload = {Critical.class}) // ❌ Does NOT work
     private String email;

  ```
- Reason: `Must implement Payload`.

---

## 📝 Key Points / Reminders

- Payload is optional — not needed for normal validations.  
- Marker interface (e.g., ErrorLevel) is optional, only for organization.  
- Inner classes inside class must be static; if it is inside interfaces they are implicitly static.
- If nested/inner class, `always use ParentClass.NestedClass.class` in annotation.
- If top-level class, you can use reference only with the class directly (Critical.class).
- You can name interfaces and nested classes whatever you like; there are no defaults like High, Low, Severity.
- Always `wrap payload` in curly `braces {}`, even if it’s a single class because payload attribute is `an array`.
- Always reference the class in annotation
