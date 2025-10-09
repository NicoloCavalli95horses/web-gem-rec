Hello,
I am a researcher at INRIA (France) and I am conducting an empirical study on web security. In a test environment, my team and I have developed a protocol that has allowed us to discover authorization vulnerabilities in real applications. We believe that https://www.busuu.com your has severe BAC (Broken Access Control) issues: by accessing and modifying the state of React components and feeding the front end with controlled HTTP responses, we were able to access any level in any order, including pronunciation levels and premium lessons available exclusively to premium users. We were also able to download a PDF of vocabulary (a premium feature). We believe these types of attacks, even if they only target the front-end code, can unlock almost all premium features, making a subscription unnecessary.

We intend to support applications that may be affected by these vulnerabilities.
Following, details about the vulnerability:

# Setup
- Install React Dev Tool
> https://chromewebstore.google.com/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi)
- Install Tweak
> https://chromewebstore.google.com/detail/tweak-mock-and-modify-htt/feahianecghpnipmhphmfgmpdodhcapi)

Note that tampering with HTTP responses can be done at lower level by leveraging instrumented browsers engines (e.g., Burp Suite, OWASP ZAP). See https://dl.acm.org/doi/10.1145/3355369.3355599 for more details about JavaScript instrumentation.

# Prerequisites
- Having a regular account (no premium)


# Step to reproduce

## Arbitrary access to levels (premium feature) -> see video `arbitrary_level_access.mp4` and `premium_lessons.mp4`
- From the dashboard, select a blocked level
- Open the Browser Development Tools -> open the tab `Components` provided by the React Dev Tool extension
- Using the component selector, select the component that renders the blocked level of your choice
- The component should be named `TimelineRegular`. Among its props, there is the property `unlocked: false`
- On the browser console, run `$r.props.unlocked = true`. This will grab the selected component, access its props and modify the value of the property `unlocked`
- In order for the component to be updated, it has to be mounted and remounted again. Click on the selected locked level, close the popup and try again
- The blocked level is now accessible
- The level is fully executable but the results are not saved (temporary privilege elevation)

## Download vocabulary (premium feature) -> see video `download_vocabulary.mp4`
- Launch any unlocked level
- The download feature is blocked, as it is a premium feature
- Open the Browser's Developer Tools -> open the `Network` tab -> make sure the log button is enabled, then clear the network logs, then reload the page. A new list of network logs will appear.
- The first log identifies the sensitive HTTP response -> Open the response named `graphql`
- Copy the HTTP request to Tweak, making sure it is labeled POST
- Copy the HTTP request body to Tweak, changing `isPremium` to `true` and adding `roles: [6]`
- Reload the page
- The download feature is enabled

# Possible solutions

- Ensure that critical features (such as accessing levels in any order, or performing actions regarding the user credit) are fully handled server-side, independently of any frontend state.
- Disable React Dev Tool access in production (best practice)

We are available to provide further details or assistance. We kindly ask for an official acknowledgement of these vulnerabilities and an estimate of the fix time.

Attached, you will find three demonstration videos highlighting the vulnerabilities. We declare that we have not disclosed these vulnerabilities to any third party. Thank you for your time and consideration.

We hope to hear from you soon.

Nicolo'