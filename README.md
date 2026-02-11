# 🛠️ Data Validation in Spring Boot

Data Validation in Spring Boot is the process of validating user input automatically using Jakarta Bean Validation annotations (like `@NotNull`, `@Size`, `@Email`) instead of writing manual `if conditions`, ensuring that the data meets required constraints before processing.

---

- 📌 **Types of Validation Annotations**  

 - 1️⃣ Default (Built-in) Annotations 
 - 2️⃣ Custom Validation Annotations

- **Common Attributes** (Almost all annotations contain these):

  -  **message** → Custom error message  
  -  **groups** → Used for grouping validations  
  -  **payload** → Used for custom payload objects

### 📝 1. message
- Used to define custom error message  
- If validation fails, this message will be shown

### 🔧 2. groups
- Used to apply validation conditionally  
- Different validations can run for different scenarios  
- Mostly used in Create / Update operations

### 🎯 3. payload
- Used to attach custom metadata information  
- Rarely used in normal applications  
- Mostly used for severity level classification  

---

## 🟢 Null Handling Annotations

### ❌ `@NotNull`
- Checks value must not be null  
- Allows empty or blank values  
- Attributes: message, groups, payload

### ⚪ `@Null`
- Checks value must be null  
- Attributes: message, groups, payload

### 🟠 `@NotEmpty`
- Checks value must not be null or empty ("")  
- Allows blank space (" ")  
- Attributes: message, groups, payload

### 🟡 `@NotBlank`
- Checks value must not be null, empty, or blank  
- Attributes: message, groups, payload  

---

## 🔤 String Validations

### 📧 `@Email`
- Checks valid email format (local + @ + domain)  
- Does not verify real domain existence  
- Attributes: message, groups, payload, regexp, flags

### 🔣 `@Pattern`
- Checks value matches given regex  
- Attributes:  
  - regexp → Regular expression  
  - message  
  - groups  
  - payload  
  - flags

### 📏 `@Size`
- Checks length/size within range  
- Attributes:  
  - min  
  - max  
  - message  
  - groups  
  - payload  

---

## 🔢 Number Validations

### 🔼 `@Min`
- Minimum numeric value allowed  
- Attributes: value, message, groups, payload

### 🔽 `@Max`
- Maximum numeric value allowed  
- Attributes: value, message, groups, payload

### ➕ `@Positive`
- Value must be > 0  
- Attributes: message, groups, payload

### ➖ `@Negative`
- Value must be < 0  
- Attributes: message, groups, payload

### ⬆️ `@PositiveOrZero`
- Value must be ≥ 0  
- Attributes: message, groups, payload

### ⬇️ `@NegativeOrZero`
- Value must be ≤ 0  
- Attributes: message, groups, payload

### 🔢 `@DecimalMin`
- Minimum decimal value  
- Attributes:  
  - value  
  - inclusive  
  - message  
  - groups  
  - payload

### 🔢 `@DecimalMax`
- Maximum decimal value  
- Attributes:  
  - value  
  - inclusive  
  - message  
  - groups  
  - payload

### #️⃣ `@Digits`
- Checks number of digits  
- Attributes:  
  - integer → digits before decimal  
  - fraction → digits after decimal  
  - message  
  - groups  
  - payload  

---

## 📅 Date & Time Validations

### ⏳ `@Past`
- Must be past date  
- Attributes: message, groups, payload

### 🕒 `@PastOrPresent`
- Past or present date  
- Attributes: message, groups, payload

### ⏭️ `@Future`
- Must be future date  
- Attributes: message, groups, payload

### ⏮️ `@FutureOrPresent`
- Future or present date  
- Attributes: message, groups, payload  

---

## ✅ Boolean Validations

### ✔️ `@AssertTrue`
- Value must be true  
- Attributes: message, groups, payload

### ❎ `@AssertFalse`
- Value must be false  
- Attributes: message, groups, payload
