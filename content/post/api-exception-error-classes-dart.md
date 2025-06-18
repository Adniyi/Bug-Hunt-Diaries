---
date: "2025-06-17T23:54:50-07:00"
draft: false
title: "Api Exception Error Classes Dart"
cover:
  image: "post/img/error-image.jpg"
  alt: "500 ERROR"
---

When working with APIs we often use try-catch blocks to catch any error that may occur, it is the standard practice and I for one cant count the number of time it has saved me from my application just crashing.

One day a colleague of mine told me that he isn't a big fan of using try-catch and refrains from using it unless its absolutely necessary, he stated that he would rather check whatever error that may occur that use a try-catch.

He went on to say that try-catch aren't really memory efficient and should be used when you know exactly what error your expecting.

This got me thinking about a lot things, but mostly about how I have misused try-catch till now. I would always use the generic `Exception` class when using `try-catch`.

So I decided to change how I used `try-catch` only using it when I am going after a specific exception.

But what `Exception` am I looking out for exactly, To answer this I decided to find out about the different exception classes available in `Dart` (which is the language of flutter and what I am currently learning).

In this article I will discuss the main exception classes provided by the `http` package in dart which is a package for handling http requests.

---

### 🛠️ **1. `TimeoutException`**

Occurs when a request exceeds the set timeout duration.

```dart
import 'dart:async';

try {
  final response = await http.get(Uri.parse(url)).timeout(Duration(seconds: 5));
} on TimeoutException catch (e) {
  print("Timeout: $e");
}
```

---

### 🛠️ **2. `SocketException`**

Thrown when there is a network error — for example, no internet connection.

```dart
import 'dart:io';

try {
  final response = await http.get(Uri.parse(url));
} on SocketException catch (e) {
  print("No Internet: $e");
}
```

---

### 🛠️ **3. `HttpException`**

Thrown for malformed HTTP responses or unexpected status codes (though this is rare with `http` package unless you manually throw it).

```dart
import 'dart:io';

try {
  final response = await http.get(Uri.parse(url));
  if (response.statusCode != 200) {
    throw HttpException("Unexpected status: ${response.statusCode}");
  }
} on HttpException catch (e) {
  print("HTTP error: $e");
}
```

---

### 🛠️ **4. `FormatException`**

Occurs when the response body is not in the expected format, such as invalid JSON.

```dart
try {
  final response = await http.get(Uri.parse(url));
  final data = jsonDecode(response.body); // Might throw FormatException
} on FormatException catch (e) {
  print("Invalid response format: $e");
}
```

---

### 🛠️ **5. `ClientException` (from `http` package)**

This is thrown internally in the `http` package when there's an issue creating the request (e.g., bad URL).

```dart
import 'package:http/http.dart';

try {
  final response = await get(Uri.parse("invalid-url"));
} on ClientException catch (e) {
  print("Client exception: $e");
}
```

---

### 🧰 **General Catch Block**

For unknown or uncategorized exceptions, you can use a general `catch`:

```dart
try {
  final response = await http.get(Uri.parse(url));
} catch (e) {
  print("Unexpected error: $e");
}
```

---

### ✅ Best Practice

You can use multiple `on` clauses to handle specific exceptions, followed by a general `catch`:

```dart
try {
  final response = await http.get(Uri.parse(url)).timeout(Duration(seconds: 10));
  final data = jsonDecode(response.body);
} on SocketException {
  print("No Internet connection.");
} on TimeoutException {
  print("Request timed out.");
} on FormatException {
  print("Bad response format.");
} catch (e) {
  print("Something went wrong: $e");
}
```

### 🔚 Conclusion

try-catch is has simplified the way we (especially me) handle errors that can occur in development, by providing classes that conform to different types of errors when making network calls.

Even though, it tends to get misused a lot by people who don't know how to use it properly, because sometimes throughing a generic exception doesn't tell you the full story of what the error is and what caused it.

Taking the time to understand and handle exceptions the right way can make your apps more stable and much easier to debug. Instead of catching every error with a generic `Exception`, using specific error types helps keep your code cleaner, easier to manage, and more reliable in the long run.
