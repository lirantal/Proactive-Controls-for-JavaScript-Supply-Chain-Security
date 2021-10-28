# Proactive Controls for JavaScript Supply Chain Security

The following are a list of secure architectural patterns, security best practices, and secure guidelines that promote defense mechanisms for supply chain security in the JavaScript ecosystem.

## Secure project management

### Include a SECURITY.TXT

- ✅ Include a `SECURITY.md` file in your repository's root directory to communicate your security policy, your disclosure policy and how to overall responsibly disclose security bugs.
- ❌ Not having a `SECURITY.md` file may result in security reports publicly disclosed which puts users at risk

GitHub features built-in support for `SECURITY.md` files which exposes that policy via the GitHub UI at _Security->Security Policy_

### Enforce 2FA for project members

TBD 

### Use GitHub environments to separate secrets

TBD

## Secure use of Build Automation Systems

### Deterministic dependency tree

- ✅ Always install dependency trees with deterministic version resolutions
- ❌ Do not perform semantic versioning resolution of dependencies during build time, as this will potentially install malicious or compromised versions.

Depending on your package manager of choice, ensure you are following this guideline with:
* `yarn install --frozen-lockfile`.
* `npm ci`.

### Pin trusted versions of CI plugins

CI systems and their ecosystem may provide 3rd-party plugins to assist in the build process. Using these creates an exposure to supply chain security concerns.

- ✅ Always use an immutable version tag such as a stable release (`v1`) or a version hash 
- ❌ Do not use a `latest` or `master` version modifiers, which could resolve in CI run-time to unvetted and potentially malicious versions of artifacts that they pull in during the build.

## Secure use of Package Registries

### Avoid publishing secrets to the npm registry

- ✅ Use the `files` key in `package.json` to create an allowlist of files that will be packaged and published to the registry.
- ❌ Do not use both `.gitignore` and `.npmignore` as the latter will be used as the source of truth and everything not specified in it, will be published to the registry.

### Enable Multi-factor Authentication 

- ✅ Enable 2FA for your account
- ✅ Use automation tokens CI-based automated publishing process of npm packages

## Use organization scopes

- ✅ Register and manage your npm packages under an organization scope reserved namespace

## Secure Package Management

### Never allow pre/post install hooks

- ✅ Use the `ignore-scripts` configuration option in your `.npmrc` to disallow the user of arbitrary command execution by 3rd party dependencies

### Use secure versions of npm

npm version 7 and above have incorporated several security-enhanced workflows:

- ✅ The `ignore-scripts` configuration option is applied only for install-lifecycle hooks.

### Enforce trusted package origins

- ✅ Enforce trusted sources of package origins to avoid lockfile injection. See [lockfile-lint](https://github.com/lirantal/lockfile-lint)


### Review open source package health

- ✅ Consult the current maintenance state of a package before installing it
- ❌ Do not blindly install dependencies from the CLI using copy/paste from untrusted sources, and be cautious of misspelling, both of which can expose you to threats of typosqautting attacks.

## Secure use of Private Proxies

The use of private proxies as a middle tier to interface with upstream open registries
is a well known and commonly used practice within organizations and enterprsies. These proxies
are often used in order to speed up artifacts downloads, save bandwidth, and serve as a local
storage of private packages and libraries which has IP associated with it and the business
wishes to not put in the public domain.

These private proxies can be a commercially deployed product, like JFrog's Artifactory, or an
open source community project like Verdaccio.

A security research disclosure in 2021, dubbed [Dependency Confusion](https://medium.com/@alex.birsan/dependency-confusion-4a5d60fec610)
pointed weaknesses in how private packages are managed, which may allow malicious actors to
replace an internal private package with a malicious one in the attacker's control.

The security best practices layed out in this section are with regards to mitigating, and detecting
occurences of Dependency Confusion, and they should be followed by software engineers and operations
teams whom are managing artifacts proxies.

### Use .npmrc


- ✅ Use a `.npmrc` configuration file in the project's source code repository
- ✅ Provision a global `.npmrc` with internal proxy
- ❌ Do not rely on `NPM_CONFIG_REGISTRY`
- ❌ Do not rely on `npm config set registry <http://...>`

ℹ️ **Details**

It may sound obvious but put all the protections in place
so that a default install of your project doesn’t expose you to dependency confusion

The core issue resides with the concern of not having the proper private npm proxy configuration.
If a developer, or a CI system, misses on having this configuration, then you’re potentially vulnerable.

Also - 
If you can provision and enforce a global .npmrc file for all users and their development environment it will
help to place the internal proxy at it (when configured correctly) so that no accidental public registry metadata fetching will happen
in case a developer forgets to add the internal proxy as part of the configuration, and then runs npm install, which will potentially
have you vulnerable to dependency confusion

