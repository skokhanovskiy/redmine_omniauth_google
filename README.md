## Redmine omniauth google

This plugin is used to authenticate Redmine users using Google's OpenId Connect provider.

### Installation:

Download the plugin and install required gems:

```console
cd /path/to/redmine/plugins
git clone https://github.com/tasogarei/redmine_omniauth_google.git
cd /path/to/redmine
bundle install
```

Restart the app
```console
touch /path/to/redmine/tmp/restart.txt
```

### Registration

To authenticate via Google you must first register your redmine instance via the Google Cloud Console

* Go to the [registration](https://cloud.google.com/console) link.
* Click your Project's name
* Click "APIs & Services" -> "Credentials"
* Click "Create credentials" -> "OAuth ClientId"
* Select "Web Application" as the Platform
* Type a Name for the application, e.g. "My Redmine"
* Type a Authorized redirect URIs for the callback url("https://mydomain.com/redmine/oauth2callback"), where "mydomain.com/redmine" is the domain / path for your redmine instance. *** The plugin will not work without this setting ***
* Click "Create"
* Save the Client ID and Client Secret for the configuration of the Redmine plugin (see below)

### Configuration

* Login as a user with administrative privileges. 
* In top menu select "Administration".
* Click "Plugins"
* In plugins list, click "Configure" in the row for "Redmine Omniauth Google plugin"
* Enter the Сlient ID & Client Secret shown when you registered your application via Google Cloud Console.
* Check the box near "Oauth authentication"
* Click Apply. 
 
Users can now to use their Google Account to log in to your instance of Redmine.

Additionaly
* Setup value Autologin in Settings on tab Authentification

### Other options

By default, all user email domains are allowed to authenticate through Google.
If you want to limit the user email domains allowed to use the plugin, list one per line in the  "Allowed domains" text box.

For example:

```text
onedomain.com
otherdomain.com
```

With the above configuration, only users with email addresses on the domains "onedomain.com" and "otherdomain.com" will be allowed to acccess your Redmine instance using Google OAuth.

### Authentication Workflow

1. An unauthenticated user requests the URL to your Redmine instance.
2. User clicks the "Login via Google" buton.
3. The plugin redirects them to a Google sign in page if they are not already signed in to their Google account.
4. Google redirects user back to Redmine, where the Google OAuth plugin's controller takes over.

One of the following cases will occur:
1. If self-registration is enabled (Under Administration > Settings > Authentication), user is redirected to 'my/page'
2. Otherwse, the an account is created for the user (referencing their Google OAuth2 ID). A Redmine administrator must activate the account for it to work.

### The operation has been confirmed
3.3.2
3.4.3

### More
If the Google's icon does not meet Redmine's theme, you can change it.
Default icon is `btn_google_signin_dark_normal_web.png`.

Change File : `/app/views/hooks/_view_account_login_bottom.html.erb`

#### Dark icon
```
<%= stylesheet_link_tag 'buttons', :plugin => 'redmine_omniauth_google' %>

<% if Setting.plugin_redmine_omniauth_google['oauth_authentification'] %>
  <%= link_to oauth_google_path(:back_url => back_url) do %>
    <%= button_tag :class => 'button-login' do %>
      <%= image_tag('/plugin_assets/redmine_omniauth_google/images/btn_google_signin_dark_normal_web.png', :class => 'button-login-icon', :alt => l(:login_via_google)) %>
    <% end %>
  <% end %>
<% end %>
```
#### Light icon
```
<%= stylesheet_link_tag 'buttons', :plugin => 'redmine_omniauth_google' %>

<% if Setting.plugin_redmine_omniauth_google['oauth_authentification'] %>
  <%= link_to oauth_google_path(:back_url => back_url) do %>
    <%= button_tag :class => 'button-login' do %>
      <%= image_tag('/plugin_assets/redmine_omniauth_google/images/btn_google_signin_light_normal_web.png', :class => 'button-login-icon', :alt => l(:login_via_google)) %>
    <% end %>
  <% end %>
<% end %>
```