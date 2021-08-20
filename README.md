# Proactive Controls for JavaScript Supply Chain Security

## Secure use of Package Managers

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


✅ Use a `.npmrc` configuration file in the project's source code repository
✅ Provision a global `.npmrc` with internal proxy

❌ Do not rely on `NPM_CONFIG_REGISTRY`
❌ Do not rely on `npm config set registry <http://...>`

ℹ️ Details

It may sound obvious but put all the protections in place
so that a default install of your project doesn’t expose you to dependency confusion

The core issue resides with the concern of not having the proper private npm proxy configuration.
If a developer, or a CI system, misses on having this configuration, then you’re potentially vulnerable.

Also - 
If you can provision and enforce a global .npmrc file for all users and their development environment it will
help to place the internal proxy at it (when configured correctly) so that no accidental public registry metadata fetching will happen
in case a developer forgets to add the internal proxy as part of the configuration, and then runs npm install, which will potentially
have you vulnerable to dependency confusion

