# APIv2: Resources

## Address

A [User](/#user)'s address, normally supplied when requested in the pledge flow.

### Address Attributes

Attribute | Type | Description
--------- | ---- | -----------
addressee | string | Full recipient name. Can be null.
line_1 | string | First line of street address. Can be null.
line_2 | string | Second line of street address. Can be null.
postal_code | string | Postal or zip code. Can be null.
city | string | City.
state | string | State or province name. Can be null.
country | string | Country.
phone_number | string | Telephone number. Can be null.
created_at | string (UTC ISO format) | Datetime address was first created.
confirmed | boolean | `true` if the address was confirmed after creation.
confirmed_at | string (UTC ISO format) | When this address was last confirmed, set by `confirmed` action attribute. Can be null.

### Address Relationships

Relationship | Type | Description
------------ | ---- | -----------
user | [User](/#user) |
campaigns | array[[Campaign](/#campaign)] |

## Benefit

A benefit added to the [Campaign](/#campaign), which can be added to a tier to be delivered to the patron.

### Benefit Attributes

Attribute | Type | Description
--------- | ---- | -----------
title | string | Benefit display title.
description | string | Benefit display description. Can be null.
benefit_type | string | Type of benefit, such as `custom` for creator-defined benefits. Can be null.
rule_type | string | A rule type designation, such as `eom_monthly` or `one_time_immediate`. Can be null.
created_at | string (UTC ISO format) | Datetime this benefit was created.
delivered_deliverables_count | integer | Number of deliverables for this benefit that have been marked complete.
not_delivered_deliverables_count | integer | Number of deliverables for this benefit that are due, for all dates.
deliverables_due_today_count | integer | Number of deliverables for this benefit that are due today specifically.
next_deliverable_due_date | string (UTC ISO format) | The next due date (after EOD today) for this benefit. Can be null.
tiers_count | integer | Number of tiers containing this benefit.
is_deleted | boolean | `true` if this benefit has been deleted.

### Benefit Relationships

Relationship | Type | Description
------------ | ---- | -----------
tiers | array[[Tier](/#tier)] |
deliverables | array[[Deliverable](/#deliverable)] |
campaign | [Campaign](/#campaign) |

## Campaign v2

The creator's page, and the top-level object for accessing lists of members, tiers, etc.

### Campaign Attributes

Attribute | Type | Description
--------- | ---- | -----------
summary | string | The creator's summary of their campaign. Can be null.
creation_name | string | The type of content the creator is creating, as in "`vanity` is creating `creation_name`". Can be null.
pay_per_name | string | The thing which patrons are paying per, as in "`vanity` is making $1000 per `pay_per_name`". Can be null.
one_liner | string | Pithy one-liner for this campaign, displayed on the creator page. Can be null.
main_video_embed | string |  Can be null.
main_video_url | string |  Can be null.
image_url | string | Banner image URL for the campaign.
image_small_url | string | URL for the campaign's profile image.
thanks_video_url | string | URL for the video shown to patrons after they pledge to this campaign. Can be null.
thanks_embed | string |  Can be null.
thanks_msg | string | Thank you message shown to patrons after they pledge to this campaign. Can be null.
is_monthly | boolean | `true` if the campaign charges per month, `false` if the campaign charges per-post.
has_rss | boolean | Whether this user has opted-in to rss feeds.
has_sent_rss_notify | boolean | Whether or not the creator has sent a one-time rss notification email.
rss_feed_title | string | The title of the campaigns rss feed.
rss_artwork_url | string | The url for the rss album artwork. Can be null.
is_nsfw | boolean | `true` if the creator has marked the campaign as containing nsfw content.
is_charged_immediately | boolean | `true` if the campaign charges upfront, `false` otherwise. Can be null.
created_at | string (UTC ISO format) | Datetime that the creator first began the campaign creation process. See `published_at`.
published_at | string (UTC ISO format) | Datetime that the creator most recently published (made publicly visible) the campaign. Can be null.
pledge_url | string | Relative (to patreon.com) URL for the pledge checkout flow for this campaign.
patron_count | integer | Number of patrons pledging to this creator.
discord_server_id | string | The ID of the external discord server that is linked to this campaign. Can be null.
google_analytics_id | string | The ID of the Google Analytics tracker that the creator wants metrics to be sent to. Can be null.
earnings_visibility | string | Controls the visibility of the total earnings in the campaign.

### Campaign Relationships

Relationship | Type | Description
------------ | ---- | -----------
tiers | array[[Tier](/#tier)] |
creator | [User](/#user) |
benefits | array[[Benefit](/#benefit)] |
goals | array[[Goal](/#goal)] |

## Deliverable

The record of whether or not a patron has been delivered the benefitthey are owed because of their member tier.

### Deliverable Attributes

Attribute | Type | Description
--------- | ---- | -----------
completed_at | string (UTC ISO format) | When the creator marked the deliverable as completed or fulfilled to the patron. Can be null.
delivery_status | string | One of 'not_delivered', 'delivered', 'wont_deliver'.
due_at | string (UTC ISO format) | When the deliverable is due to the patron.

### Deliverable Relationships

Relationship | Type | Description
------------ | ---- | -----------
campaign | [Campaign](/#campaign) |
benefit | [Benefit](/#benefit) |
member | [Member](/#member)[Member](/#member)[Member](/#member)[Member](/#member) | The member who has been granted the deliverable.
user | [User](/#user) | The user who has been granted the deliverable. This user is the same as the member user.

## Goal

A funding goal in USD set by a creator on a campaign.

### Goal Attributes

Attribute | Type | Description
--------- | ---- | -----------
amount_cents | integer | Goal amount in USD cents.
title | string | Goal title.
description | string | Goal description. Can be null.
created_at | string (UTC ISO format) | When the goal was created for the campaign.
reached_at | string (UTC ISO format) | When the campaign reached the goal. Can be null.
completed_percentage | integer | Equal to (pledge_sum/goal amount)*100, helpful when a creator
                wants to hide their pledge sum but still wants their goals to be cash based

### Goal Relationships

Relationship | Type | Description
------------ | ---- | -----------
campaign | [Campaign](/#campaign) | The campaign trying to reach the goal

## Media

A file uploaded to patreon.com, usually an image.

### Media Attributes

Attribute | Type | Description
--------- | ---- | -----------
file_name | string | File name.
size_bytes | integer | Size of file in bytes.
mimetype | string | Mimetype of uploaded file, eg: "application/jpeg".
state | string | Upload availability state of the file.
owner_type | string | Type of the resource that owns the file.
owner_id | string | Ownership id (See also `owner_type`).
owner_relationship | string | Ownership relationship type for multi-relationship medias.
upload_expires_at | string (UTC ISO format) | When the upload URL expires.
upload_url | string | The URL to perform a POST request to in order to upload the media file.
upload_parameters | string | All the parameters that have to be added to the upload form request.
download_url | string | The URL to download this media. Valid for 24 hours.
created_at | string (UTC ISO format) | When the file was created.
metadata | string | Metadata related to the file. Can be null.

## Member

The record of a user's membership to a campaign. Remains consistent across months of pledging.

### Member Attributes

Attribute | Type | Description
--------- | ---- | -----------
patron_status | string |  Can be null.
is_follower | boolean | The user is not a pledging patron but has subscribed to updates about public posts.
full_name | string | Full name of the member user.
email | string | The member's email address. Requires the `campaigns.members[email]` scope.
pledge_relationship_start | string (UTC ISO format) | Datetime of beginning of most recent pledge chainfrom this member to the campaign. Pledge updates do not change this value. Can be null.
lifetime_support_cents | integer | The total amount that the member has ever paid to the campaign. `0` if never paid.
currently_entitled_amount_cents | integer | The amount in cents that the member is entitled to.This includes a current pledge, or payment that covers the current payment period.
last_charge_date | string (UTC ISO format) | Datetime of last attempted charge. `null` if never charged. Can be null.
last_charge_status | string | The result of the last attempted charge. Possible values are `['Paid', 'Declined', 'Deleted', 'Pending', 'Refunded', 'Fraud', 'Other', null]`. The only successful status is `Paid`. `null` if never charged. Can be null.
note | string | The creator's notes on the member.
will_pay_amount_cents | integer | The amount in cents the user will pay at the next pay cycle.

### Member Relationships

Relationship | Type | Description
------------ | ---- | -----------
address | [Address](/#address) | The member's shipping address that they entered for the campaign.Requires the `campaign.members.address` scope.
campaign | [Campaign](/#campaign) | The campaign that the membership is for.
currently_entitled_tiers | array[[Tier](/#tier)] | The tiers that the member is entitled to. This includes a current pledge, or payment that covers the current payment period.
user | [User](/#user) | The user who is pledging to the campaign.

## OAuthClient

A client created by a developer, used for getting OAuth2 access tokens.

### OAuthClient Attributes

Attribute | Type | Description
--------- | ---- | -----------
client_secret | string |
name | string |
description | string |
author_name | string |
domain | string |
version | integer |
icon_url | string |
privacy_policy_url | string |  Can be null.
tos_url | string |  Can be null.
redirect_uris | string |
default_scopes | string |

### OAuthClient Relationships

Relationship | Type | Description
------------ | ---- | -----------
user | [User](/#user) |
campaign | [Campaign](/#campaign) |

## Tier

A membership level on a campaign, which can have benefits attached to it.

### Tier Attributes

Attribute | Type | Description
--------- | ---- | -----------
amount_cents | integer | Monetary amount associated with this tier (in U.S. cents).
user_limit | integer | Maximum number of patrons this tier is limited to, if applicable. Can be null.
remaining | integer | Remaining number of patrons who may subscribe, if there is a `user_limit`. Can be null.
description | string | Tier display description.
requires_shipping | boolean | `true` if this tier requires a shipping address from patrons.
created_at | string (UTC ISO format) | Datetime this tier was created.
url | string | Fully qualified URL associated with this tier.
patron_count | integer | Number of patrons currently registered for this tier.
post_count | integer | Number of posts published to this tier. Can be null.
discord_role_ids | string | The discord role IDs granted by this tier. Can be null.
title | string | Tier display title.
image_url | string | Full qualified image URL associated with this tier. Can be null.
edited_at | string (UTC ISO format) | Datetime tier was last modified.
published | boolean | `true` if the tier is currently published.
published_at | string (UTC ISO format) | Datetime this tier was last published. Can be null.
unpublished_at | string (UTC ISO format) | Datetime tier was unpublished, while applicable. Can be null.

### Tier Relationships

Relationship | Type | Description
------------ | ---- | -----------
campaign | [Campaign](/#campaign) | The campaign the tier belongs to.
tier_image | [Media](/#media) | The image file associated with the tier.
benefits | array[[Benefit](/#benefit)] | The benefits attached to the tier, which are used for generating deliverables

## User v2

The Patreon user, which can be both patron and creator.

### User Attributes

Attribute | Type | Description
--------- | ---- | -----------
email | string | The user's email address. Requires certain scopes to access. See the scopes section of this documentation.
first_name | string | First name. Can be null.
last_name | string | Last name. Can be null.
full_name | string | Combined first and last name.
is_email_verified | boolean | `true` if the user has confirmed their email.
vanity | string | The public "username" of the user. patreon.com/<vanity> goes to this user's creator page. Non-creator users might not have a vanity. Can be null.
about | string | The user's about text, which appears on their profile. Can be null.
image_url | string | The user's profile picture URL, scaled to width 400px.
thumb_url | string | The user's profile picture URL, scaled to a square of size 100x100px.
can_see_nsfw | boolean | `true` if this user can view nsfw content. Can be null.
created | string (UTC ISO format) | Datetime of this user's account creation.
url | string | URL of this user's creator or patron profile.
like_count | integer | How many posts this user has liked.
hide_pledges | boolean | `true` if the user has chosen to keep private which creators they pledge to. Can be null.
social_connections | string | Mapping from user's connected app names to external user id on the respective app.

### User Relationships

Relationship | Type | Description
------------ | ---- | -----------
memberships | array[[Member](/#member)] | Usually a zero or one-element array with the user's membership to the token creator's campaign, if they are a member. With the `identity.memberships` scope, this returns memberships to ALL campaigns the user is a member of.
campaign | [Campaign](/#campaign) |

## Webhook

Webhooks are fired based on events happening on a particular campaign.

### Webhook Attributes

Attribute | Type | Description
--------- | ---- | -----------
triggers | collection | List of events that will trigger this webhook.
uri | string | Fully qualified uri where webhook will be sent (e.g. https://www.example.com/webhooks/incoming).
paused | boolean | `true` if the webhook is paused as a result of repeated failed attempts to post to `uri`. Set to `false` to attempt to re-enable a previously failing webhook.
last_attempted_at | string (UTC ISO format) | Last date that the webhook was attempted or used.
num_consecutive_times_failed | integer | Number of times the webhook has failed consecutively, when in an error state.
secret | string | Secret used to sign your webhook message body, so you can validate authenticity upon receipt.

### Webhook Relationships

Relationship | Type | Description
------------ | ---- | -----------
client | [OAuth-Client](/#oauthclient) | The client which created the webhook
campaign | [Campaign](/#campaign) | The campaign whose events trigger the webhook.