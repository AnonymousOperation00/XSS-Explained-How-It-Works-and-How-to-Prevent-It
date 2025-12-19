# XSS-Explained-How-It-Works-And-How-To-Prevent-It

Cross-Site Scripting (XSS) is a web security vulnerability that allows an attacker to inject and execute malicious client-side code, typically JavaScript, in another user's browser.

XSS occurs when an application includes user-controlled data in its output without proper validation, sanitization, or encoding. As a result, the browser interprets the injected content as trusted code and executes it in the context of the vulnerable website.

Because the code runs in the victim's browser, an XSS vulnerability can be exploited to:

- **manipulate** page content
- **perform actions** on behalf of the user
- **steal sensitive data**, such as session identifiers
- **abuse application logic**

XSS vulnerabilities often occur when applications use `innerHTML`
to insert user-controlled data directly into the page.

When untrusted input is stored in the DOM as HTML, the browser
parses and executes it as part of the page, which can lead to unexpected script execution.

To reduce the risk of XSS attacks, user-supplied data should not be treated as HTML.

Instead, secure alternatives such as `textContent` should be used, which insert data
as plain text rather than executable code.

Some APIs and functions are particularly dangerous when used with untrusted input
and should be avoided unless proper sanitization and encoding are implemented.

In JavaScript, the following properties and methods are common sources of XSS
when they are used to render user-controlled data:

- `innerHTML`
- `outerHTML`
- `document.write()`
- `document.writeln()`
- `insertAdjacentHTML()`
- `setAttribute()` (when setting event handlers or URLs)
- `eval()` and similar dynamic code execution functions

These APIs interpret input as HTML or executable code and may allow injected
content to become part of the page.

In PHP-based applications, XSS vulnerabilities often occur when user input is
output directly to the page without proper escaping or encoding.

Common risky patterns include:
- `echo` or `print` used with unescaped user input
- `printf()` / `sprintf()` with user-controlled data
- `include` / `require` with unsanitized parameters
- Direct output of database content that was never properly encoded

To mitigate these risks, output encoding functions such as `htmlspecialchars()`
or `htmlentities()` should be applied before rendering data in HTML contexts.

XSS prevention should always be based on context-aware output encoding rather
than relying solely on input filtering.

❌ Reflected XSS vulnerability example

```php
<?php
// User input is reflected directly into the HTML response
$name = $_GET['name'] ?? '';
echo "<p>Hello $name</p>";
```

✅ Fixed code no longer vulnerable to reflected xss

<?php
$name = $_GET['name'] ?? '';
echo "<p>Hello " . htmlspecialchars($name, ENT_QUOTES, 'UTF-8') . "</p>";
