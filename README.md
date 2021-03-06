# VMO<sup>3

## Business/Technical Challenge
Have you ever set up your out of office message in Outlook, but forgot to enable your voice mail or change the greeting? 
Maybe you've returned to the office and forgot to disable that voice mail telling everyone that you are away.

VMO<sup>3</sup> allows users to enable their out of office message once and have it reflected across multiple platforms. 

## Proposed Solution

Our application will integrate O365 Exchange Out of Office alerting with Cisco Unity Connect (UCXN) and 
Cisco Unified Communication Manager (CUCM). Users can set their OoO alert in email and have it automatically enabled in 
voice mail. They will also receive an Webex Teams message that contains the number of the inbound caller and a separate 
Webex Teams message that contains the transcription of the message the caller leaves. 

<img src= "https://github.com/clintmann/vmo3/blob/master/images/vmo3_concept_image.gif" />


### Cisco Products Technologies/ Services

Our solution will leverage the following Cisco technologies

* [Cisco Unity Connection (UCXN)](https://www.cisco.com/c/en/us/products/unified-communications/unity-connection/index.html)
* [Cisco Unified Communications Manager (CUCM)](https://www.cisco.com/c/en/us/products/unified-communications/unified-communications-manager-callmanager/index.html)
* [Cisco WebEx Teams](https://www.webex.com/products/teams/index.html)

## Team Members

* Chris Bogdon <cbogdon@cisco.com> - Trans PNC
* Marty Sloan <masloan@cisco.com> - Midwest Atlantic Enterprise
* Clint Mann <climann2@cisco.com> - PA Select - Commercial Accounts


## Solution Components

VMO<sup>3</sup> is made up of three microservices. Below is an architectural diagram of the components. The diagram also 
shows, at a high level, what each module interacts with. 

Both <a href="https://github.com/clintmann/vmo3/tree/master/outlook-monitor#vmo3-outlook-monitor">outlook-monitor</a> 
and <a href="https://github.com/clintmann/vmo3/tree/master/vmo-mediator#vmo3---vmo-mediator">vmo-mediator</a> 
were written in Python and 
<a href="https://github.com/sloan58/vmo3-uc/tree/f3960683fe10bcfa4c5f0d814df3a197f397191a#vmo3-uc-connector">uc-connector</a>
was written in PHP. 

This solution uses:
* The Microsoft Graph development platform
* An Office 365 mailbox
* [Cisco Unity Connection (UCXN)](https://www.cisco.com/c/en/us/products/unified-communications/unity-connection/index.html)
* [Cisco Unified Communications Manager (CUCM)](https://www.cisco.com/c/en/us/products/unified-communications/unified-communications-manager-callmanager/index.html)
* [Cisco WebEx Teams](https://www.webex.com/products/teams/index.html)
* Amazon Polly for text to speech translation
 
<img src= "https://github.com/clintmann/vmo3/blob/master/images/vmo3_architecture.gif" />


## Usage

The solution is comprised of three components.

## <a href="https://github.com/clintmann/vmo3/tree/master/vmo-mediator#vmo3---vmo-mediator">vmo-mediator</a> 
This is is a Python Flask microservice that really is the module that connects everything together. 
It is this interface taht is used to choose which users to 
start and stop monitoring. It also acts as a "bridge" to pass user information gathered from 
<a href="https://github.com/clintmann/vmo3/tree/master/outlook-monitor#vmo3-outlook-monitor">outlook-monitor</a> to the
<a href="https://github.com/sloan58/vmo3-uc/tree/f3960683fe10bcfa4c5f0d814df3a197f397191a#vmo3-uc-connector">uc-connector</a>
. 

## <a href="https://github.com/clintmann/vmo3/tree/master/outlook-monitor#vmo3-outlook-monitor">outlook-monitor</a> 
This is a Python Flask microservice that listens for 
the <a href="https://github.com/clintmann/vmo3/tree/master/vmo-mediator#vmo3---vmo-mediator">vmo-mediator</a> 
microservice to specify an Active Directory user to either start or stop monitoring. Outlook-monitor 
queries Microsoft Graph to determine if a users exists in Microsoft Azure Active Directory and then checks the 
Office 365 Outlook/Exchange Automatic Replies (Out of Office) status of that user. 

## <a href="https://github.com/sloan58/vmo3-uc/tree/f3960683fe10bcfa4c5f0d814df3a197f397191a#vmo3-uc-connector">uc-connector</a>
This connector is written in PHP and acts as a gateway to trigger UC-specific activities when a user's Out of Office 
status has changed. It is responsible for the text to speech translation as well as posting incoming call information and
voice mail message to Webex Teams. 

Once all components are up and running, the user will interface exclusively with the 
<a href="https://github.com/clintmann/vmo3/tree/master/vmo-mediator#vmo3---vmo-mediator">vmo-mediator</a> dashboard.

## Installation

Since there are three distinct modules required, the detailed information for installation is included in the 
documentation links provided in the next section.


## Documentation

More detailed information and documentation can be provided in the following links:

* [outlook-monitor](outlook-monitor/README.md)
* [vmo-mediator](vmo-mediator/README.md)
* [uc-connector](https://github.com/sloan58/vmo3-uc/blob/master/README.md)

## Video Demonstration URL
https://youtu.be/9nXl2Y5sS5c

## License

Provided under Cisco Sample Code License, for details see [LICENSE](./LICENSE.md)

## Code of Conduct

Our code of conduct is available [here](./CODE_OF_CONDUCT.md)

## Contributing

See our contributing guidelines [here](./CONTRIBUTING.md)
