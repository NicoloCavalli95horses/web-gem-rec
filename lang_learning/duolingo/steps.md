Good afternoon,
I am a researcher at INRIA (France) and I am conducting an empirical study on web security. In a test environment, my team and I have developed a protocol that has allowed us to discover authorization vulnerabilities in real applications. We believe that your web application (https://www.duolingo.com/learn) is affected by BAC (Broken Access Control) issues: by hijacking the state of React components and feeding the front end with controlled HTTP responses, we were able to unlock any lesson in any order (implicitly bypassing ads) and were able to manipulate user credit, unlocking infinite “hearts” (premium features).
We intend to support you in fixing this issue. We only ask for an estimated resolution time and the ability to officially acknowledge the vulnerability. Following, a full text walkthrough explaining the vulnerabilities we found:

# Setup
- Install Tweak
> https://chromewebstore.google.com/detail/tweak-mock-and-modify-htt/feahianecghpnipmhphmfgmpdodhcapi)

Note that HTTP response tampering can be done at a lower level by exploiting instrumented browser engines (e.g., Burp Suite, OWASP ZAP).

# Prerequisites
- Have a standard account (not premium).

# Step to reproduce

## Arbitrary Level Access (Premium Feature) -> see `arbitrary_level_access.mp4`
1. From the dashboard, select a blocked level.
2. Open the browser's Developer Tools -> open the `Network` tab -> make sure the log button is enabled, then clear the network logs, then reload the page. A new list of network logs will appear.
3. Open the first response that includes the query `fields=acquisition` (example below)
> https://www.duolingo.com/2017-06-30/users/877574425?fields=acquisitionSurveyReason,adsConfig,animationEnabled,betaStatus,blockedUserIds,blockerUserIds,canUseModerationTools,classroomLeaderboardsEnabled,courses,creationDate,currentCourseId,email,emailAnnouncement,emailAssignment,emailAssignmentComplete,emailClassroomJoin,emailClassroomLeave,[more content...]
4. Copy the HTTP request to Tweak, making sure it's labeled GET. Do not copy the last token, which identify the current session
5. Copy the HTTP request body to Tweak, changing `locked` to `active`
6. Reload the page
7. All levels are accessible in any order

## Unlimited Hearts -> see `unlimited_hearts.mp4`
1. Repeat the previous step from 1 to 4.
2. Copy the HTTP request body into Tweak, changing the value of `hearts`
3. Reload the page
4. Each time a mistake is made, 1 heart will be subtracted from the attacker's defined amount

# Possible solutions

- Ensure that critical features (such as accessing levels in any order, or performing actions regarding the user credit) are fully handled server-side, independently of any frontend state.

We are available to provide further details or assistance. Attached, you will find three demonstration videos highlighting the vulnerabilities. We declare that we have not disclosed these vulnerabilities to any third party. Thank you for your time and consideration.
We hope to hear from you soon.

Nicolo'
