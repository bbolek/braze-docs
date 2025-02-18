---
nav_title: Punchh
article_title: Punchh
page_order: 5
description: "This article outlines the partnership between Braze and Punchh, a loyalty and engagement platform, enabling you to sync data across the two platforms. Data published in Braze will be available for segmentation and can sync user data back into Punchh via webhook templates setup in Braze. "
alias: /partners/punchh/
page_type: partner
search_tag: Partner

---

# Punchh

> [Punchh](https://punchh.com/) is an industry-leading loyalty and engagement platform that enables brands to deliver omnichannel customer loyalty programs both in-store and digitally. 

The Braze and Punchh integration allows you to sync data for gifting and loyalty purposes across the two platforms. Data published in Braze will be available for segmentation and can sync user data back into Punchh via Braze webhooks.

## Prerequisites

| Requirement | Description |
|---|---|
| Punchh Account | You need an active Punchh account to take advantage of this partnership. |
| Braze REST API key | A Braze REST API key with `users.track` permissions. <br><br> This can be created within the **Braze Dashboard > Developer Console > REST API Key > Create New API Key**. |
| Braze REST Endpoint | [Your REST Endpoint URL][6]. Your endpoint depends on the Braze URL for your instance. |
{: .reset-td-br-1 .reset-td-br-2}

## Integration

Punchh offers several endpoints available to Braze customers to help add external IDs to the Punchh platform using the following Punchh API endpoints. Once the external IDs have been added, create an adapter in Punchh, provide your Braze credentials, and select which events you'd like to sync. Next, you can take the Punchh segment ID and use it to build a Punchh webhook to trigger customer syncing in a Canvas journey.

### Available Events to Sync {#available-events-to-sync}
1. Guest
2. Loyalty Check-in
3. Gift Check-in
4. Redemption
5. Rewards
6. Transaction Notifications
7. Marketing Notifications

{% alert note %}
Reference Punchh documentation on what sample payloads for these available events may look like. 
{% endalert %}

### Step 1: Set up external ID ingestion endpoints

External IDs from Braze can be added using the following endpoints for new and existing Punchh users.

{% alert important %}
The `external_source` and `external_source_id` field values must be unique to Punchh and not associated with existing profiles.
{% endalert %}

1. New Punchh users<br>
Create new users in Punchh with a Punchh sign-up endpoint using the `external_source` and `external_source_id` fields. Punchh allows external identifiers to be sent with a user profile via one of the following sign-up endpoints:
- [Mobile Signup API](https://developers.punchh.com/mobile-apis/users/mobile-sign-up)
- [SSO Signup API](https://developers.punchh.com/sso-online-apis/single-sign-on/sso-signup)<br><br>
2. Existing Punchh users <br>
Update `external_source_id` for existing Punchh users. Punchh allows external identifiers to be added to a profile via a user API update endpoint: 
- [Mobile User Update](https://developers.punchh.com/mobile-apis/users/mobile-update-user-profile)
- [SSO User Update](https://developers.punchh.com/sso-online-apis/single-sign-on/sso-update-user-information)
- [Dashboard User Update](https://developers.punchh.com/platform-functions-apis/users/dashboard-users-update)
<br><br>
{% tabs local %}
{% tab User sign-up API example %}
This example allows you to send external identifiers with a user profile at sign-up time. This is done by sending `external_source` as “customer_id” and `external_source_id` as “556644557788334412” as a string data type.

```json
curl --location --request POST 'https://sandbox.punchh.com/api2/mobile/users' \
--header 'Content-Type: application/json' \
--header 'x-pch-digest: eac5b04cbf7362c5359a4c259cf8fc18941646bf2e11bfe46be0031ffaa1100b' \
--header 'Accept-Timezone: Etc/UTC' \
--header 'Accept: application/json' \
--header 'Accept-Language: en' \
--data-raw '{
    "client":"1533b61caecafea4303aa1f4bad8321d6d8e7a843593e4a0e0024ae0d30b",
    "user" : {
      "email": "example@braze.com",
      "password": "p@ssw0rd",
      "first_name":"Amit",
      "last_name":"K",
      "terms_and_conditions":"true",
      "anniversary":"2014-02-02",
      "zip_code":"94497",
      "birthday":"2004-02-02",
      "external_source":"customer_id",
      "external_source_id":"556644557788334412"
      }
}'
```
{% endtab %}
{% tab User update API example %}
This example allows you to update external identifiers with a user profile. This is done by sending `external_source` as “customer_id” and `external_source_id` as “556644557788334412” as a string data type.

```json
curl --location --request PUT 'https://sandbox.punchh.com/api2/mobile/users' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Accept-Language: en' \
--header 'x-pch-digest: 953d896eebfdb5a84aacb9d1b8eaae1fa0cd710b68bcd3b2324415ac40fee99c' \
--header 'Authorization: Bearer c90b819bf962db9882eeac6993b57c0a22816ecad0e5229b27320d63' \
--data-raw '{
    "client":"1533b61caecafea4303aa1f4bad8321d6d8e7a843593e4a0e0024ae0d30b",
    "user": {
        "external_source":"customer_id",
        "external_source_id":"556644557788334412"
    }
}'
```
{% endtab %}
{% endtabs %}

{% alert note %}
Platform configuration: To enable external identifiers in Punchh, from the Punchh dashboard, navigate to **Cockpit > Dashboard > External User Identifier**.
{% endalert %}

### Step 2: Braze adapter setup in Punchh

To set up the Braze and Punchh integration:

1. In the Punchh dashboard, navigate to **Cockpit > Dashboard > Major Features > Enable Webhook Management** and toggle on **Enable Webhook Management**.<br><br>
2. Next, enable adapters by navigating to **Settings > Webhooks Manager > Configurations > Show Adapters Tab** and toggle on **Show Adapters Tab**.<br><br>
3. Navigate to **Webhooks Manager** under the **Settings** tab, select the **Adapters** tab, and click **Create Adapter**. <br><br>![][1]<br><br>
4. Fill in the adapter name, description, and admin email. Select **Braze** as your adapter and provide your Braze REST API endpoint and Braze API key.<br><br>
5. Next, select the available events you would like to enable. A list of these events can be found in [Available events to sync](#available-events-to-sync).<br><br>![][3]<br><br>
6. Click **Submit** to enable the webhook.

### Step 3: Create Punchh webhook in Braze

The Braze and Punchh integration allows you to leverage Braze webhook capabilities to create Punchh segments:

1. Create a custom segment in Punchh and note the `custom_segment_id` present in the Punchh segment dashboard URL. <br><br>For example, the following page is located at `www.dashboard.punchhtest.com/segments/11646`. The number "11646" at the end of this link is the `custom_segment_id`.<br><br>![][5]<br><br>
2. Create a webhook campaign in Braze using the Punchh endpoint for adding a user to a custom segment as the webhook URL. Here, you can provide the `custom_segment_id` and `user_id` as key-value pairs.<br><br>![][4]<br><br>
3. Once the webhook is saved, it can be triggered in Canvas to sync users, as shown in the following image:<br><br>![An example of syncing users using the saved webhook due to Braze and Punchh integration.][7]

For more information on how webhooks are used at Braze, check out [Creating a webhook]({{site.baseurl}}/user_guide/message_building_by_channel/webhooks/creating_a_webhook/). 

## Use case campaigns

{% tabs local %}
{% tab Signup %}
#### Sign-up campaign

When utilizing the Braze configuration for a sign-up campaign with an offer attached, a sign-up gifting campaign will need to be configured within Punchh and a welcome message in Braze. 

Punchh recommends that an execution delay be added to the sign-up campaign, so Braze can first trigger the welcome message based on the guest event. If you want to send a follow-up message informing the user that they have been gifted, you can trigger this based on the reward event.

In the case of a sign-up campaign, all signed up can be used for the segment; therefore, a custom Braze segment will not be required.

Punchh configurations required:
- Campaign: Sign-up 
- Segment: All signed up
- Reward: Customer choice
Events required:
- Reward event
- Guest event
Considerations:
- Execution delay, recommend that the guest add a 5–10-minute delay

![A user segment is configured in punch and then guests sign up for a loyalty program. After this the guest event if triggered and the Braze messaging campaign is triggered. Next, the Punchh signup gifting campaign is triggered after 10 minutes, triggereing the reward event and optional follow-up message.]({% image_buster /assets/img/punchh/usecase3.png %})

{% endtab %}
{% tab Mass offer %}

#### Mass offer campaign

When utilizing a mass offer campaign for gifting, a mass offer campaign will need to be configured within Punchh and a messaging campaign in Braze.

If you want to utilize a Braze segment for your campaign or send communication from Braze before gifting guests in the Punchh Platform, then a [custom Punchh segment]({{site.baseurl}}/partners/message_orchestration/channel_extensions/loyalty/punchh/#step-3-create-punchh-webhook-in-braze) will be required for the Punchh gifting campaign. 

Creating the segment of users to receive this offer in Braze is only recommended when using attributes unavailable within Punchh. Otherwise, Punchh segmentation can be used, and the Braze messaging campaign will be created as an action-based campaign triggered by the users receiving their reward (the reward event triggered by Punchh).

Punchh Configurations Required:
- Campaign: Mass offer
- Segment: Custom list or customer choice
- Reward: Customer choice

**If using Punchh for segmentation and gifting, and Braze for messaging:**<br>
For example, a $2 off reward is sent to a segment configurable within Punchh with messaging sent through Braze.<br>
![A user segment can be configured in Punchh, and users receive a gift through a Punchh mass offer campaign. Next, a reward event is trieggered and then the Braze messaging campaign is triggered.]({% image_buster /assets/img/punchh/usecase4.png %}){: style="max-width:125%;"}

**If using Braze segmentation and messaging, but Punchh for gifting:**<br>
For example, a $2 off reward and messaging sent to a segment with attributes not available in Punchh.<br>
![A user segment can be configured in Braze, and then a message sent from Braze to Braze segment. Next, the users sent to Punchh custom segment through a Braze webhook with segment and user ID. After this the user recieve a gift through Punchh mass offer campaign with a custom segment. After this the reward event is triggered.]({% image_buster /assets/img/punchh/usecase5.png %}){: style="max-width:125%;"}

**If using Braze segmentation and Punchh for gifting and/or messaging:**<br>
For example, a $2 off reward is sent to a segment with attributes not available in Punchh, but no messaging is required, or the messaging can be sent through Punchh (note that all guests must be present in Punchh).<br>
![A user segment can be configured in Braze, and the users sent to Punchh custom segment through a Braze webhook with segment and user ID. After this the user recieve a gift through Punchh mass offer campaign with a custom segment. After this the reward event is triggered.]({% image_buster /assets/img/punchh/usecase6.png %}){: style="max-width:75%;"}


{% endtab %}
{% tab Recurring mass offer %}

#### Recurring mass offer campaign

When utilizing a recurring mass offer campaign for gifting, a mass offer campaign will need to be configured within Punchh and a messaging campaign set up in Braze. A Punchh custom segment will be required if the customer wants to use Braze segmentation (only recommended if utilizing attributes unavailable within Punchh). Otherwise, Punchh segmentation can be used, and the Braze messaging campaign will be triggered based on the reward event.

Punchh configurations required:
- Campaign: Recurring mass offer
- Segment: Custom list or customer choice
- Reward: Customer choice
Considerations:
- Campaign IDs and campaign names are sent to Braze as an event property on the event. If you want to use a Punchh campaign identifier in Braze to further filter the audience receiving the campaign, the campaign name must be used since the campaign IDs will change daily.

{% endtab %}
{% tab Post check-in offer %}

#### Post check-in offer campaign

When utilizing a post check-in offer campaign that does not first notify customers, the campaign will gift (optional messaging) and trigger any notification within Braze. Therefore, a post check-in offer campaign must be configured within Punchh; however, a custom list is not required. Instead, you can choose the segment you would like within Punchh. 

Punchh configurations required:
- Campaign: Post check-in offer
- Segment: Customer choice
- Reward: Customer choice

For example, a surprise and delight Braze campaign is sent to a segment available in Punchh, thanking guests for visiting and rewarding them with $2 off their next visit.

![An qualifying user segment can be configured within Punchh, and a qualifying user checks-in and recieved a gift through a Punchh post check-in campaign. After this a reward event is triggered and the recall message is sent notifying guests of the reward sent from Braze.]({% image_buster /assets/img/punchh/usecase2.png %})

{% endtab %}
{% tab  Anniversary %}

#### Anniversary Campaign 

When utilizing an anniversary campaign, a user will first be gifted for their anniversary from the Punchh campaign. This gifting (reward event) will trigger the messaging campaign within Braze that notifies the user of the gifting. Therefore, a custom list is not required. Instead, you can choose the segment and anniversary setting within Punchh.

Punchh configurations required:
- Campaign: Anniversary campaign
- Segment: Customer choice
- Reward: Customer choice
Considerations:
- Gifting month of sign-up
- Lifespan duration (How long is the birthday reward valid?)
- Recurring campaigns, schedule required 

![An optional segment can be created within Punchh, and a qualifying user receives a reward through a Punchh anniversay campaign. After this a reward event is triggered and the recall message is sent notifying guests of the reward sent from Braze.]({% image_buster /assets/img/punchh/usecase1.png %})

{% endtab %}
{% tab Recall %}

#### Recall campaign

When targeting users based on inactivity, a recall Campaign can be used. The customer can create the segment and campaign within Punchh but utilize Braze for messaging.

If you want to use segmentation created in Braze, a [custom Punchh segment]({{site.baseurl}}/partners/message_orchestration/channel_extensions/loyalty/punchh/#step-3-create-punchh-webhook-in-braze) based on inactivity can be attached to a recurring mass offer campaign.

Punchh configurations required:
- Campaign: Recall campaign
- Segment: Customer choice
- Reward: Customer choice
Considerations:
- Campaign runs on a schedule

![An optional segment can be created within Punchh, and a qualifying user receives a reward through a Punchh recall campaign. After this a reward event is triggered and the recall message is sent notifying guests of the reward sent from Braze.]({% image_buster /assets/img/punchh/usecase.png %})

{% endtab %}
{% endtabs %}


[1]: {% image_buster /assets/img/punchh/punchh1.png %}
[2]: {% image_buster /assets/img/punchh/punchh2.png %}
[3]: {% image_buster /assets/img/punchh/punchh3.png %}
[4]: {% image_buster /assets/img/punchh/punchh4.png %}
[5]: {% image_buster /assets/img/punchh/punchh5.png %}
[7]: {% image_buster /assets/img/punchh/punchh6.png %}
[6]: {{site.baseurl}}/api/basics?redirected=true#endpoints