# Server-side template injection

## Attack tree

```text
1 Use native template syntax to inject a malicious payload into a template
    1.1 Detect
        1.1.1 Fuzz template
        1.1.2 Check plaintext context
        1.1.3 Check code context
    1.2 Identify
        1.2.1 Submit invalid syntax
        1.2.2 Manually test different language-specific payloads
    1.3 Exploit
```

## Example

## Notes

* Template engines are widely used by web applications to present dynamic data via web pages and emails. Unsafely embedding user input in templates enables Server-Side Template Injection, a frequently critical vulnerability that is extremely easy to mistake for Cross-Site Scripting (XSS), or miss entirely. Unlike XSS, Template Injection can be used to directly attack web servers' internals and often obtain Remote Code Execution (RCE), turning every vulnerable application into a potential pivot point.
* If fuzzing a template by injecting a sequence of special characters commonly used in template expressions, such as `${{<%[%'"}}%\`, raises an exception, it indicates that the injected template syntax is potentially being interpreted by the server in some way. This is one sign that a vulnerability to server-side template injection may exist. 
* Plaintext context check: If requesting a URL such as: `http://vulnerable-website.com/?username=${7*7}`, renders `49` in the response, this shows that the mathematical operation is being evaluated server-side, and is vulnerable. 
* Code context check: first establish that the parameter doesn't contain a direct XSS vulnerability by injecting arbitrary HTML into the value, the try and break out of the statement using common templating syntax and attempt to inject arbitrary HTML after it. If renders blank, either not vulnerable, or the wrong language was used for the test. If NOT renders blank, vulnerable.
* Templating languages use very similar syntax that is specifically chosen not to clash with HTML characters. As a result, it can be relatively simple to create probing payloads to test which template engine is being used. Submitting invalid syntax is often enough because the resulting error message will tell exactly what the template engine is, and sometimes even which version. 

## Tools

* [BurpSuite](https://portswigger.net/burp)

## Resources

* [PortSwigger: Server-side template injection](https://portswigger.net/web-security/server-side-template-injection)
