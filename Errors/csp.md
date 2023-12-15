# Contenst Security Policy of your website blocks the use of 'eval' in JavaScript.

# This is a security feature of Content Security Policy.

# You can disable Content Security Policy by adding the following meta tag to your HTML:

# <meta http-equiv="Content-Security-Policy" content="script-src 'self' 'unsafe-inline';" />

# For more information, see https://go.microsoft.com/fwlink/?linkid=2127425

The Content Security Policy (CSP) prevents the evaluation of arbitrary strings as JavaScript to make it more difficult for an attacker to inject unathorized code on your site.
To solve this issue, avoid using eval(), new Function(), setTimeout([string], ...) and setInterval([string], ...) for evaluating strings.
If you absolutely must: you can enable string evaluation by adding unsafe-eval as an allowed source in a script-src directive.
⚠️ Allowing string evaluation comes at the risk of inline script injection.
