# ExtendedMailingStats CiviCRM extension

This extension provides extended summary reports on CiviCRM mailings

## Installation

You can install this from github by doing:

    cd [your civicrm extensions directory]
    git clone git@github.com:mc0e/civicrm-extendedmailingstats.git au.org.greens.extendedmailingstats

You will then need to enable the extension module in civicrm by going to using the civicrm admin page at:

    'Administer' -> 'Customise data and Screens' -> 'Manage Extensions'

You then go to:

    'Reports' -> 'Create Reports from Templates'

And you'll see  'Extended Mailing Stats' in the list.  

## Cron job setup

Stats are collected by a cron job rather than when the report is collected.  The cron job 
needs to be set up to run a command along the lines of:

    drush -r /var/www/example.org/htdocs -l example.org -u 1 civicrm-api extendedmailingstats.cron auth=0 -y

The cron job should run as the web server user.

## Data Provided

 * Mailing Name 
 * Date Created 
 * Start Date 
 * End Date 
 * recipients 
 * delivered 
 * bounced 
 * opened 
 * unique_opened
 * unsubscribed 
 * opted_out
 * forwarded 
 * clicked_total 
 * clicked_unique 
 * trackable_urls 
 * clicked_contribution_page 
 * contributions_48hrs_count 
 * contributions_48hrs_total 
 * gmail_recipients 
 * gmail_delivered 
 * gmail_opened 
 * gmail_clicked_total 
 * gmail_clicked_unique

### Mailing Name

The name of the mailing.

### Date Created

The date on which the Mailing was created. It may be sent significantly later, or not yet sent.

### Start Date

The time when the first non-test mailing job associated with the mailing began to be processed. Note that in other reports, CiviCRM uses the scheduled time as the start date. This will typically be a little earlier than the start date of the first mailing job, as the job doesn't start until the next cron run.

### End Date

The time when the last non-test mailing job associated with the mailing finished being processed.

### Recipients

The number of recipients the email was configured to be delivered to. This is recorded explicitly in the database in relation to the mailing, and this figure will not subsequently change as membership of target groups changes.

### Delivered

The number of actual deliveries made which are associated with non-test jobs for the mailing.

### Bounced

The number of deliveries which bounces, as recorded by CiviCRM.

### Trackable_urls

The number of different trackable URLs in the mailing.  Note that this counts the URLs, not the links, whereas it's not unusual for the same URL to appear more than once. The CiviCRM data structure does not allow us to distinguish betwen these links.

### Opened

CiviCRM embeds a transparent single pixel image in sent emails, so that whenever that email is displayed, the image is loaded from CiviCRM, and an event is recorded, identifying the mailing, the recipient the email was sent to and a timestamp.

For each mailing, this field currently records the number of such events. If the same user opens the email more than once, it is counted multiple times.

### Unique Opened

Reports the number of unique recipients who opened the email.

### Clicked_total, Clicked_unique

CiviCRM substitutes URLs in sent emails, so that whenever a user clicks a link, it goes to a CiviCRM URL where the click is recorded, and the user is then redirected to the target URL. The event record identifies the mailing, the user the email was sent to, the target URL and a timestamp.

It is not possible to distinguish between clicks on different links to the same URL which may appear in the same mailing.

Total Clicks records the number of click events. If the same user clicks through from the same email more than once, it is counted multiple times, and if they click multiple links those are all counted also.

Unique Clicks records the number of recipients who clicked on a link.

### gmail_recipients, gmail_delivered, gmail_opened, gmail_clicked_total, gmail_clicked_unique

Exactly as for the non-gmail equivalents, except that reporting is only for those users with "@gmail.com" email addresses.

### Unsubscribed

CiviCRM records unsubscribe events associated with the mailing. This column counts all unsubscribe events. 

Note that there are times where multiple unsubscribe events are being recorded in the database which involve the same mailing and the same user.

### Forwarded

CiviCRM records when users use its mechanism for forwarding emails. Some CiviCRM mailings includea link for this, but not all. Where such a link is present, and used, the event is recorded, much as for other events.

### clicked_contribution_page

This is a count of the total number of click events associated with the mailing where the URL involved is recognised as being for a CiviCRM contribution page. It matches on the URL path (after the domain name) starting with "/CiviCRM/contribute/transact" .

If one user clicks multiple times it's counted more than once.

Note that where the mailing contains a URL with a different format which redirects to the CiviCRM page, this extension cannot count clicks on those links.

### contributions_48hrs_count

For each mailing, we identify the recognisable contribution page URLs in the mailing, and we count the contributions associated with those URLs which are made in the 48 hour period after the Scheduled time of the mailing by a contact who recieved the mailing.

It's not all htat unusual for a single contact to donate more than once.  Each such contribution is counted.

### contributions_48hrs_total

The contributions associated with the mailing are collected as for contributions_48hrs_count, but this column gives the total ammount contributed.

## Authorship

This module was developed by Andrew McNaughton <andrew@mcnaughty.com> for the Australian Greens <http://greens.org.au>

