# Mimedefang
authentik does not have a mechanism to change the _From address_ which means that you cannot use your brand as part of the e-mail message that will be sent to users as the register for your authenticator.

Postfix and Sendmail both support the use of Milters which is a type of mail filter that provides a framework to inspect and modify email as it's being processed.  Mimedefang is one such milter that will allow you to modify the sender during the e-mail delivery process.

Note: This assumes that you host your own SMTP server.

- Works with Authentik version 2025.8.3
- Requires installation of mimedefang
  * **Ubuntu** :
```
% sudo apt install mimedefang
```
  * **RHEL** :
```
% sudo dnf install mimedfang
```
  * **Other distributions** : _Search the many resources available_

## mimedefang-filter
Update the mimedefang-filter file to include the appropriate rewriting rules.
In my case, I was using the default sender 'authentik@localhost' so I essentially used the following rule:

```
if (sender == 'authentik@localhost')
{
  then, if subject matches brand string
  {
    Change sender to preferred From address
  }
}
```

The specific filter in the mimedefang-filter looks like this

```
sub filter_end {
    my($entity) = @_;

    my $head = $entity->head;
    my $subject = $head->get('Subject');
    my $from = $head->get('From');

    if (defined $from && $from =~ /authentik/)
    {
      if (defined $subject && $subject =~ /^\[BRAND 1 TEXT\].*/)
      {
        change_sender('info@brand1.com');
        action_change_header('From','BRAND 1 <info@brand1.com>');
      }
      elsif (defined $subject && $subject =~ /^\[BRAND 2 TEXT\].*/)
      {
        change_sender('info@brand2.com');
        action_change_header('From','BRAND 2 <info@brand2.com>'); 
      }
    }
    action_add_header('X-Authentik-From',$from);

    md_graphdefang_log('mail_in');
}
```

## Configure / Start mimedefang
Using the mimedefang and your OS distribution, you will need to configure, enable and start mimedefang so that it can begin doing rewrites for you.

## Configure SMTP server to use mimedefang
You will also need to enable mimedefang as a milter within your SMTP server application.  This should be pretty straightforward with postfix and sendmail.
