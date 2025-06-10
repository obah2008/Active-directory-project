
# Shuffle integration
In this section of the project, I'll be creating a shuffle workflow, and integrating that with our AD environment. but before we get into that, let's understand what an SOAR is and why shuffle is so Important.

## SOAR(Security Orchestration Automation and Response)
Security Orchestration, Automation, and Response (SOAR) refers to a set of tools that help security teams build automated workflows and pipelines for detecting, analyzing, and responding to security events.  
These tools connect different security systems (like SIEMs, firewalls, and communication platforms), allowing teams to automate repetitive tasks.

## Shuffle 
Shuffle is a SOAR tool that allows users to automate processes and create workflow pipeline using drag and drop blocks, without having to write a single line of code.

### Shuffle setup
- The first step to setting up shuffle is to create a new [shuffle](https://shuffler.io/) account.
- With my shuffle account setup, I can make a new workflow which I'll be calling Obah-AD
- On the new workflow dashboard we can now create a new webhook(A way for one application to send data to another when a specific event occurs)
- Copy the webhook URI(uniform resource Identifier)
- head over to the Splunk dashboard, Navigate to the alert we created in the [SIEM integration]() section. Edit the Alert and add the copied URI to webhook section
- Headback to the shuffle dashboard and start the webhook.
