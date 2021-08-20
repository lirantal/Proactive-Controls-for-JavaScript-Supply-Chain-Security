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

