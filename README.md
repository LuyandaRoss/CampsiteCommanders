# Campsite Commander

## Student Information

**Student Name:** [Your Name]
**Student Number:** [Your Student Number]
**Module:** Introduction to Mobile Application Development
**Assessment:** Practical Assignment
**Application Name:** Campsite Commander

---

# Project Overview

Campsite Commander is an Android application developed using Kotlin in Android Studio. The purpose of the application is to assist outdoor adventurers in organizing and managing camping equipment and supplies.

The application allows users to:

* Add camping gear items.
* Categorize equipment.
* Store quantities of each item.
* View a detailed checklist.
* Calculate the total number of packed items.
* Navigate between multiple screens.

The project demonstrates the use of:

* Arrays and ArrayLists
* Loops
* User Input
* Screen Navigation
* Splash Screen
* Error Handling
* Android User Interface Design

---

# Features

## Splash Screen

When the application starts:

* Displays the Campsite Commander logo.
* Shows the application title.
* Remains visible for 3 seconds.
* Automatically navigates to the Main Screen.

---

## Main Screen

The Main Screen allows users to:

* Enter an Item Name.
* Enter a Category.
* Enter a Quantity.
* Add items to the packing list.
* View the total number of packed items.
* Navigate to the Detailed View Screen.

Example items included:

| Item         | Category | Quantity | Comment             |
| ------------ | -------- | -------- | ------------------- |
| Tent         | Shelter  | 1        | 4-person waterproof |
| Marshmallows | Food     | 3        | For S'mores         |
| Flashlight   | Safety   | 2        | Check batteries     |

---

## Detailed View Screen

The Detailed View Screen displays:

* Item Name
* Category
* Quantity
* Comments

The screen uses loops to display all stored items.

A "Back to Base" button allows the user to return to the Main Screen.

---

# Arrays Used

The application stores data using parallel arrays (ArrayLists).

```kotlin
private val itemNames = ArrayList<String>()
private val categories = ArrayList<String>()
private val quantities = ArrayList<Int>()
private val comments = ArrayList<String>()
```

These arrays store corresponding information for each camping item.

---

# Loop Implementation

The application uses loops to:

## Calculate Total Items

```kotlin
var total = 0

for (qty in quantities) {
    total += qty
}
```

## Display Detailed Information

```kotlin
for (i in items.indices) {

    displayText +=
        "Item: ${items[i]}\n" +
        "Category: ${categories!![i]}\n" +
        "Quantity: ${quantities!![i]}\n" +
        "Comments: ${comments!![i]}\n\n"
}
```

---

# Error Handling

The application validates user input before adding items.

```kotlin
if (
    edtItem.text.isEmpty() ||
    edtCategory.text.isEmpty() ||
    edtQuantity.text.isEmpty()
) {
    Toast.makeText(
        this,
        "Please complete all fields",
        Toast.LENGTH_SHORT
    ).show()
}
```

This prevents incomplete data from being stored.

---

# Screen Navigation

The application contains three screens:

1. SplashActivity
2. MainActivity
3. DetailActivity

Navigation is performed using Android Intents.

```kotlin
val intent = Intent(this, DetailActivity::class.java)
startActivity(intent)
```

---

# Testing Results

The application was tested using the Android Emulator.

### Test Case 1

**Action:** Launch application

**Expected Result:** Splash screen displays for 3 seconds then opens Main Screen.

**Actual Result:** Passed.

### Test Case 2

**Action:** Add valid item information.

**Expected Result:** Item is added successfully.

**Actual Result:** Passed.

### Test Case 3

**Action:** Leave a field blank.

**Expected Result:** Error message displayed.

**Actual Result:** Passed.

### Test Case 4

**Action:** Open Detailed View.

**Expected Result:** All stored items displayed.

**Actual Result:** Passed.

### Test Case 5

**Action:** Press Back to Base button.

**Expected Result:** Returns to Main Screen.

**Actual Result:** Passed.

---

# GitHub Repository

GitHub Link:

[Insert GitHub Repository URL Here]

---

# Conclusion

The Campsite Commander application successfully meets all assignment requirements. The application demonstrates the use of arrays, loops, user input validation, multiple activities, navigation, and user-friendly interface design. The project provides a practical solution for organizing camping equipment while showcasing fundamental Android development concepts using Kotlin.
