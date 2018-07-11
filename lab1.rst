BIG-IQ Access Policy Manager
============================

Objective
^^^^^^^^^

BIG-IQ can create, modify, and delete APM access and per-request
policies.

WORKFLOW 1: Access Policy Review
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Navigate to Configuration Access Access Groups Boston Access Policies
Per-Session Policies Test2

|image5|

The access policy will be displayed in a new screen as shown below.
Compare the Access policy in BIG IQ with the policy in BIG IP source
device and ensure that they are exactly same.

|image6|

WORKFLOW 2: Location Specific Object Modification
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  Navigate to Configuration Access Access Groups Boston Authentication
   Active Directory

    |image7|

    LSO or Location Specific Objects are objects within an access
    profile/policy that relate to more specific geographic areas
    normally and are not shared between all devices by default to
    prevent misconfigurations. As an example, AAA servers are located in
    all office/data centers globally however if an end user is accessing
    a policy on an APM in Europe we wouldn’t want their authentication
    requests to be sent over a WAN link to some Domain Controller in
    another country and cause a tremendous delay for that user.

-  Click the check box for the FrogPolicy-olympus-ad for the BIGIP02
   device.

-  Click the Mark Shared button and accept the warning

    |image8|

    This will move the object from the device specific location to the
    Shared resources location.

-  Click on the AAA object to edit the properties

-  Change the Timeout value from 15 to 1500

-  Click Save & Close

    |image9|

    BIG IQ provides the ability to transition LSO objects to Shared
    Objects and vice versa. When an LSO object is made Shared it will
    have the same configuration across all the BIG IPs after deployment.

WORKFLOW 3: Modifying an existing APM access policy using VPE
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  Navigate to ConfigurationAccessAccess Groups

-  Select BosAG1 (or Boston)

|image10|

Click on Access Policies -> Per Session Policies:

|image11|

Select TestAccessProfile and add the following:

-  Logon page (accept default settings)

-  AD Auth using FrogPolicy-Olympus-AD

-  If AD Auth successful, your allowed access

|image12|

|image13|

|image14|

|image15|

|image16|

After modifying the access profile, go to “Deployment tab- > Evaluate &
Deploy -> Access” and Click on Create in Evaluation section. Make sure
to select BOS-vBigIP01/02 devices.

|image17|

In the evaluation section, you will be able to view the added/changed
items.

Now go forward and deploy the changes.

|image18|

|image19|

You can verify on BigIP that the access profile changes were pushed:

|image20|

WORKFLOW 4: Create a new VPN Access profile
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  Navigate to ConfigurationAccessAccess Groups

-  Select Boston

|image21|

You can see all of the access policies listed in the Per Session
Policies:

|image22|

Click Create and you will see the Access Policy creation screen. Give it
a name of “VPN-AP” and click on **Save & Close**. You can change the
view from Basic -> Advanced if you want to modify additional settings
such as timeouts, SSO, logout URI, etc..

|image23|

Then click “New” in macros and select “AD Auth and resources” template.

Edit the macro with the following settings:

AD Server = FrogPolicy-olympus-ad

Resource-> Network Access = FrogPolicy-F5\_VPN

|image24|

|image25|

|image26|

|image27|

Then add the macro into the VPE and change the ending on the
“Successful” branch to **Allow**.

|image28|

After creating and saving the access profile, go to “Deployment - >
Evaluate & Deploy -> Access”.

Click on “Create” in Evaluations, give it a name, and select
BOS-vBIGIP01/02 devices.

In the evaluation section, you will be able to view the added/changed
items.

|image29|

Click on View after the evaluation is done to view the changes in Green.

|image30|

|image31|

Then Click on Deploy and verify the new VPN Access Profile is pushed
onto the BostonAG

|image32|

|image33|

|image34|

Objective
^^^^^^^^^

WORKFLOW 5: View APM Audit logs and Dashboards 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  Navigate to Monitoring -> Audit Logs ->Access

Note: In case you do not have any data in BIG-IQ, check the active
session in Access tab in BIG-IP Boston Active cluster. If the session
shows pending (blue), restart the apmd process on the BIG-IP (bigstart
restart apmd).

|image35|

We will now walk through several different Dashboards available under
the Access portion of BIG-IQ. During this exercise we will bring
attention to several key areas of interest for Adminstrators.

Start by following along the separate menu paths below to each sub-menu
section for Access Dashboards:

-  Navigate to Monitoring -> Dashboards -> Access

   -  View Access Summary

      Notice the layout provides a great overview of usage of the entire
      Access infrastructure of devices which are currently under
      management with BIG-IQ. This single page view provide a quick
      snapshot view of license usage, Geographic access usage, top
      users, Session counts and Denied Sign-Ins. There is a time slider
      at the top of the page allowing the Admin to apply constraints of
      the time period for which the graphs and session counts should
      display. Take notice of the current Session counts and Sign-In
      Denied count, then adjust the left time slider moving it to the
      right slightly. Then adjust the right slider moving it to the left
      slightly. You will notice the session counts have changed. Now
      notice from this point an Admin could quickly drill down into
      certain areas of interest for troubleshooting. Click on the
      Sign-in Denied number to review further details. On the lower
      portion of this page you will find a list of denied sessions. You
      can see the duration of the session for the given user along with
      the username, client ip, and in this example IP-Reputation matched
      that prevented access for many of the sessions.

   -  Application Summary

      On the Application Summary screen we can see useage request for
      Top 1p apps along with Bytes In/Out details and number of Unique
      Users per application. By clicking on an application name like
      Confluence we can drill down to the details for that specific
      application.

   -  Federation -> SAML ->SP -> SP Summary

      Federation is being used more widely these days. The BIG-IP Access
      Policy Manager can perform both SAML Service Provider as well as
      Identity Provider functions. In this summary screen we see the
      Federated Assertions for foreign Identity Providers for Services
      (Applications) hosted from the Access Policy Managers in the
      organization.

      Once again an Admin can use this screen to start diagnosing issues
      like Failed Assertions by clicking the lines in that section for
      drill down details.

   -  Federation -> SAML ->IdP -> IdP Summary

      In the IdP Summary screen we see when the BIG-IP Access Policy
      Manager is acting as the Identity Provider and providing
      assertions to external Service Provider hosted applications. Same
      drill down/troubleshooting benefits can be found here for the
      Administrators of the Access environment.

   -  Remote Access -> Network Access -> Network Access Summary

      In the Network Access Summary screen you will notice something new
      between the user counts number at top and the graph below them.
      There are three TABS, Sessions, Connections, Bytes Transferred.
      You will currently be selected/presented with the Sessions Tab
      information. Click the Connections tab and review. Now click the
      Bytes Transferred tab. As of version 13.1 TMOS code that runs on
      the BIG-IPs the BIG-IQ 5.4 can display these details for reporting
      and troubleshooting and capacity usage and planning.

   -  Remote Access -> Network Access -> Network Access Usage

      This screen again is providing more detailed reporting of the
      Bytes In/Out/Transferred by given users for the Admin to utilize.

   -  Remote Access -> VDI Summary

      Many companies have implemented the use of Virtual Desktop
      Infrastructures of the years for deploying either individual
      published applications or full desktops for users. This summary
      screen provides reporting on the usage of those VDI objects being
      served through the BIG-IP Access Policy Manager working as a VDI
      Proxy for the three major flavors of VDI technology from Microsoft
      RDP, VMWare Horizon and Citrix XenApp/XenDesktop.

   -  Sessions -> Sessions Summary

      As we review the Session Summary screen you should notice under
      the ACTIVE column there are Green Dots for sessions that are
      currently active however this screen is displaying the list of all
      sessions even those denied sessions we reviewed earlier. You can
      click on the session ID to review the policy events for a given
      session.

   -  Sessions -> Active

      In this screen we are only reporting the Currently Active
      Sessions. Notice the check box to the left of eash session. You
      can click to check a box and the button above “Kill Selected
      Sessions” will be un-grayed allowing the Admin to kill the checked
      sessions. If the Admin were to click the check box in the Column
      header it would check all sessions boxes and the Kill All Sessions
      and/or Kill Selected Sessions buttons would then perform the kill
      on all sessions. In both scenarios the Admin is presented with a
      Confirmation Screen before actually killing those checked
      sessions.

   -  Sessions -> Bad IP Reputation

      In this section we can see the reported IP Reputation data for
      incoming requests to the APM Policies.

   -  Sessions -> Bowsers and OS

      This screen provide details of browser types and OSes being used
      to access the APM policies. This is great information if an
      organization has specific policies in place that stipulate which
      Browsers and OSes that support. The Admin can quickly see where
      they fall in line with those policies.

   -  Sessions -> By Geolocation

      This reporting screen provides a Heatmap displaying from where
      access attempts are being initiated from. If an organization only
      allowed or supported access from certain geographic regions this
      screen can provide quick details on possible bad actor attempts to
      the organizations Access infrastructure.

   -  Endpoint Software -> Endpoint Software Summary

      You may need to reset the Timeframe either by adjusting the
      sliders or using the Timeframe dropdown. This screen provides
      information of Endpoint Software in use by clients and detected
      via the Endpoint Inspection helper applications that run on
      clients systems and report back to the BIG-IP Access Policy
      Manager during access.

   -  Endpoint Software -> Endpoint Software Summary

      This is another great troubleshooting screen to review versions of
      client AV software.

   -  License Usage

      This screen provides an overview of the Access Policy Manager
      license usage for both Access Session licenses as well as
      Connectivity Session licenses per APM Device.

   -  User Summary

      In the user summary screen one item that can be useful to an Admin
      is the Filter Search field by Username. If your organization has a
      large community of users accessing in many different methods or
      applications the ability to filter by username and drill into
      those sessions for a specific user are helpful for troubleshooting
      issues.

Below are just a few of the screens mentioned above however taking the
time to review this Monitoring Dashboards with live data can be helpful
in getting familiar with Admin duties for Access Policy infrastructure
using the BIG-IQ Centralized Manager.

|image36|

|image37|

|image38|

|image39|

|image40|

|image41|

