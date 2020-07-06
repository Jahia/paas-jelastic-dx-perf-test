# PaaS Performance Tests Site

Jelastic manifests used to install performance tests site on Jahia (for PaaS environments).

## Manifests description

step1.yml:
- Install required modules
    - calendar-2.0.6
    - event-2.0.3
    - ldap-3.1.5
    - news-2.0.4
    - publication-2.0.2
    - remotepublish-8.2.2
    - sitemap-2.0.5
    - templates-web-blue-qa-2.0.2-SNAPSHOT
- Install Linux packages:
    - ImageMagick-devel
    - libreoffice
    - ffmpeg
- Update jahia.properties conf
- Add LDAP provider (org.jahia.services.usermanager.ldap-cloud-perf.cfg)
- Retrieve performance tests site archive and import it in Jahia
    - https://github.com/Jahia/paas-jelastic-dx-perf-test/raw/master/assets/DXPerformanceTestSite_staging_7231.zip

step2.yml:
- Pre-compile all servlets
- Update Spell-Checker index
- Restart Tomcat

## Manifests execution

First of all, step1.yml needs to be applied to the target environment (via Jelastic console or API call). 

It will trigger asynchrounous actions (site import on Jahia), so you have to wait for the processing's tomcat logs to go silent before proceeding to the next step.

Once step 1 is completely finished, you need to apply step2.yml package to the target environment (via Jelastic console or API call).

As for step 1, asychronous actions will be triggered so you also have to wait for 

If the LDAP setup is not functionnal, you need to create local test users inside Jahia Administration interface. To do so, you need to create a .csv file with the required number of users (5000 by default for performance tests) based on the following model (the first line must be present):

```
j:nodename,j:password
user1,password
user2,password
user3,password
user4,password
user5,password
user6,password
user7,password
[...]
```

Then you need to log in to your Jahia environment with root user, go to Administration → Users and Roles → Users, and Bulk create users with your newly created csv file.
