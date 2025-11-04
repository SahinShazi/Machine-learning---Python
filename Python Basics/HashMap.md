# Python Dictionary (HashMap)

## Dictionary কি জিনিস?

Dictionary মানে হলো key-value pairs এর collection। সহজ ভাষায় বলতে গেলে - একটা আসল dictionary যেমন শব্দ আর তার অর্থ রাখে, Python এর dictionary ও তেমনি key আর তার value রাখে।

উদাহরণ দিলে পরিষ্কার হবে:
- ফোনবুক - নাম (key) আর নাম্বার (value)
- দেশের রাজধানী - দেশ (key) আর রাজধানী (value)
- পণ্যের দাম - পণ্য (key) আর দাম (value)

**কেন ব্যবহার করবে?**
- খুব দ্রুত data খুঁজে পাওয়া যায়
- data organized থাকে
- real world problems solve করতে সহজ

## Dictionary এর Structure

```python
my_dict = {
    "key1": "value1",
    "key2": "value2",
    "key3": "value3"
}
```

দেখো কয়েকটা জিনিস:
- Curly brackets `{}` ব্যবহার করতে হয়
- Key আর value এর মাঝে colon `:` দিতে হয়
- প্রতিটা pair এর পরে comma `,` 
- Key unique হতে হবে (duplicate হবে না)

---

## Problem 01: দেশের রাজধানী

সবচেয়ে সহজ example দিয়ে শুরু করি।

```python
country_capitals = {
    "Bangladesh": "Dhaka",
    "India": "New Delhi",
    "Japan": "Tokyo"
}

print("Capital of Japan is:", country_capitals["Japan"])
```

**কি হচ্ছে এখানে:**
- তিনটা দেশের নাম key হিসেবে আছে
- তাদের রাজধানী value হিসেবে আছে
- Square brackets `[]` দিয়ে value access করছি
- `country_capitals["Japan"]` লিখলে "Tokyo" পাবো

আরও কিছু করতে পারো:

```python
# সব key দেখো
print(country_capitals.keys())

# সব value দেখো
print(country_capitals.values())

# নতুন দেশ যোগ করো
country_capitals["Pakistan"] = "Islamabad"
print(country_capitals)
```

---

## Problem 02: Student এর তথ্য

এবার একটু complex data রাখি।

```python
student = {
    "name": "Sahin",
    "age": 15,
    "dept": "CSE",
    "cgpa": 4.9
}

print("The CGPA is", student["cgpa"])
```

**মজার ব্যাপার:**
- Dictionary তে different type এর data রাখতে পারো
- "name" হলো string
- "age" হলো integer
- "cgpa" হলো float
- সব একসাথে থাকতে পারে!

একটা useful কাজ:

```python
# সব তথ্য একসাথে print করো
for key in student:
    print(f"{key}: {student[key]}")

# Output হবে:
# name: Sahin
# age: 15
# dept: CSE
# cgpa: 4.9
```

নিরাপদ উপায়ে value নাও (key না থাকলে error দিবে না):

```python
# এটা error দিবে যদি key না থাকে
# print(student["university"])

# এটা None return করবে
print(student.get("university"))

# Default value ও দিতে পারো
print(student.get("university", "Not specified"))
```

---

## Problem 03: Marks Filter করা

এখন একটু logic লাগবে। যাদের marks ৮০ এর বেশি তাদের নাম print করতে হবে।

```python
marks = {
    "Sahin": 85,
    "Rafi": 78,
    "Riya": 92,
    "Toma": 69
}

for name in marks:
    if marks[name] > 80:
        print(name)
```

**ব্যাখ্যা:**
- `for name in marks:` মানে সব key এর উপর loop চালাচ্ছি
- `marks[name]` দিয়ে সেই name এর marks পাচ্ছি
- যদি ৮০ এর বেশি হয় তাহলে name print করছি

Output: `Sahin` আর `Riya`

আরেকটু fancy করতে চাইলে:

```python
print("Students with 80+ marks:")
for name in marks:
    if marks[name] > 80:
        print(f"- {name}: {marks[name]}")

# Output:
# Students with 80+ marks:
# - Sahin: 85
# - Riya: 92
```

অথবা dictionary comprehension:

```python
top_students = {name: marks[name] for name in marks if marks[name] > 80}
print(top_students)
# {'Sahin': 85, 'Riya': 92}
```

---

## Problem 04: নতুন Key-Value যোগ করা

Dictionary তে নতুন জিনিস add করা অনেক সহজ।

```python
info = {
    "name": "Sahin",
    "age": 20,
    "dept": "CSE"
}

info["university"] = "DU"

print(info)
```

**এত সহজ?** হ্যাঁ! শুধু `info["university"] = "DU"` লিখলেই হলো।

আরও কিছু operation:

```python
# Update existing value
info["age"] = 21
print(info)

# একসাথে অনেকগুলো add করো
info.update({
    "city": "Dhaka",
    "cgpa": 3.85
})
print(info)

# Delete করো
del info["city"]
print(info)

# অথবা pop দিয়ে (value টাও পাবে)
cgpa = info.pop("cgpa")
print(f"Removed CGPA: {cgpa}")
print(info)
```

---

## Problem 05: Total Price হিসাব

সব product এর দাম যোগ করতে হবে।

```python
products = {
    "rice": 120,
    "milk": 300,
    "egg": 30,
    "veg": 40
}

total = 0

for item in products:
    total = total + products[item]

print("The total price is", total)
```

**একটু বুঝি:**
- `total = 0` দিয়ে শুরু করলাম
- প্রতিটা product এর দাম `total` এ যোগ করছি
- শেষে মোট দাম print করছি

Output: `490`

আরও সহজ উপায়:

```python
# values() ব্যবহার করো
total = sum(products.values())
print("Total:", total)

# অথবা একলাইনে
print("Total:", sum(products.values()))
```

Shopping list এর মতো করতে চাইলে:

```python
print("Shopping List:")
print("-" * 30)
for item, price in products.items():
    print(f"{item:10} : {price} tk")
print("-" * 30)
print(f"Total      : {sum(products.values())} tk")

# Output:
# Shopping List:
# ------------------------------
# rice       : 120 tk
# milk       : 300 tk
# egg        : 30 tk
# veg        : 40 tk
# ------------------------------
# Total      : 490 tk
```

---

## Dictionary এর Useful Methods

### keys(), values(), items()

```python
student = {"name": "Sahin", "age": 20, "dept": "CSE"}

# শুধু keys
print(list(student.keys()))      # ['name', 'age', 'dept']

# শুধু values
print(list(student.values()))    # ['Sahin', 20, 'CSE']

# দুইটা একসাথে
print(list(student.items()))     # [('name', 'Sahin'), ('age', 20), ('dept', 'CSE')]
```

### Loop এর different ways

```python
marks = {"Sahin": 85, "Rafi": 78, "Riya": 92}

# শুধু keys
for name in marks:
    print(name)

# keys আর values দুইটা
for name, mark in marks.items():
    print(f"{name} got {mark}")

# শুধু values
for mark in marks.values():
    print(mark)
```

### Check করা

```python
# Key আছে কিনা
if "Sahin" in marks:
    print("Found!")

# Value আছে কিনা
if 85 in marks.values():
    print("Someone got 85")

# Dictionary খালি কিনা
if marks:
    print("Dictionary is not empty")
```

---

## Nested Dictionary

Dictionary এর ভিতরে dictionary থাকতে পারে!

```python
students = {
    "student1": {
        "name": "Sahin",
        "age": 20,
        "marks": 85
    },
    "student2": {
        "name": "Rafi",
        "age": 21,
        "marks": 78
    }
}

# Access করো
print(students["student1"]["name"])  # Sahin
print(students["student2"]["marks"]) # 78

# Loop চালাও
for student_id, info in students.items():
    print(f"{student_id}:")
    print(f"  Name: {info['name']}")
    print(f"  Age: {info['age']}")
    print(f"  Marks: {info['marks']}")
    print()
```

---

## Real Life Example - Contact Book

চলো একটা mini contact book বানাই:

```python
contacts = {}

# Contact add করো
def add_contact(name, number):
    contacts[name] = number
    print(f"Added: {name}")

# Contact খুঁজো
def find_contact(name):
    if name in contacts:
        print(f"{name}: {contacts[name]}")
    else:
        print("Not found!")

# সব contacts
def show_all():
    if contacts:
        print("\nAll Contacts:")
        for name, number in contacts.items():
            print(f"{name}: {number}")
    else:
        print("No contacts!")

# ব্যবহার করি
add_contact("Sahin", "01712345678")
add_contact("Rafi", "01787654321")
add_contact("Riya", "01698765432")

show_all()
find_contact("Sahin")
find_contact("Toma")
```

---

## Common Mistakes

### ভুল ১: Key না থাকা

```python
student = {"name": "Sahin", "age": 20}

# ❌ Error দিবে
# print(student["dept"])

# ✅ Safely check করো
if "dept" in student:
    print(student["dept"])
else:
    print("Dept not found")

# ✅ অথবা get() ব্যবহার করো
print(student.get("dept", "Not specified"))
```

### ভুল ২: Mutable key ব্যবহার

```python
# ❌ List key হিসেবে ব্যবহার করা যায় না
# my_dict = {[1, 2]: "value"}  # Error!

# ✅ Tuple ব্যবহার করো
my_dict = {(1, 2): "value"}  # কাজ করবে
```

### ভুল ৩: Value access এ ভুল

```python
marks = {"Sahin": 85}

# ❌ ভুল syntax
# print(marks.Sahin)

# ✅ সঠিক
print(marks["Sahin"])
print(marks.get("Sahin"))
```

---

## Dictionary vs List

কখন কোনটা ব্যবহার করবে?

**List ব্যবহার করো যখন:**
- ক্রম (order) গুরুত্বপূর্ণ
- Index দিয়ে access করবে
- Duplicate values থাকবে

```python
fruits = ["apple", "banana", "cherry"]
print(fruits[0])  # apple
```

**Dictionary ব্যবহার করো যখন:**
- Key-value relationship আছে
- নাম দিয়ে access করবে
- দ্রুত lookup চাই

```python
fruit_colors = {"apple": "red", "banana": "yellow"}
print(fruit_colors["apple"])  # red
```

---

## Tips

১. **Meaningful keys ব্যবহার করো:**
```python
# ❌ খারাপ
d = {"a": 20, "b": "Sahin"}

# ✅ ভালো
student = {"age": 20, "name": "Sahin"}
```

২. **Key সবসময় immutable হতে হবে:**
- String ✅
- Number ✅
- Tuple ✅
- List ❌
- Dictionary ❌

৩. **Clear করার উপায়:**
```python
# সব delete করো
my_dict.clear()

# নতুন করে শুরু করো
my_dict = {}
```

---

## Practice Problems

১. **Word Counter**: একটা sentence এ প্রতিটা word কতবার আছে count করো
২. **Grade Book**: Student name আর marks রাখো, average বের করো
৩. **Inventory System**: পণ্যের নাম, দাম, quantity রাখো
৪. **Phone Book**: Name, number, email সব রাখো
৫. **Character Frequency**: একটা string এ প্রতিটা character কতবার আছে

---

Dictionary হলো Python এর সবচেয়ে powerful data structure গুলোর একটা। এটা ভালো করে শিখলে অনেক কাজ সহজ হয়ে যাবে। Practice করতে থাকো!

Happy Coding! 🚀