It works on my machine! I would place a confident bet every developer has said this (likely more than once!).

Chances are, you are brew updating postgres on your machine far more than your team is doing so for production. Chances are, everytime there is a new postgres version brew updates for you, you have came across the nightmere of something going astray. Going down the whirlwind of fun of reading what feels like a monolith of stackoverflows and random medium articles to help you solve. Copy and pasting commands blindly out of desperation. Finally bringing yourself to completely uninstall and reinstall postgres. Yet, the mystery is still unresolved. A pid file somewhere still hanging out...more blind copy and pasting commands that look like hieroglyphics at this point. `brew stop postgres && brew start postgres` just can not get it together. Soul crushing...

What if you always used the postgres version that matched your production environment? What if you could have multiple postgres versions as needed for your various applications? What if you could switch versions using a tool you might already use to switch ruby versions, node versions etc?

The solution is here: [asdf](https://github.com/asdf-vm/asdf). As explained in the `README`, "asdf is a CLI tool that can manage multiple language runtime versions on a per-project basis."

## Backstory:
Being a long time [rvm](https://rvm.io)'er for managing different ruby versions. I was not immediately ready to make the switch. I had yet to find anything note worthy for a node version manager. I decided to only use asdf for `nodejs` to test it out. Finally, a node version manager tool that was not trash! It worked and it worked well. Took the next big step: `rvm implode`. What a breath of fresh air! It is worth mentioning `.default-gems` is nothing short of beautiful when installing a new ruby version.

I realized there was a array of asdf [plugins](https://asdf-vm.com/#/plugins-all) I had never thought to version manage that I should actually should! Given ruby/rails are my go to loves, `postgres` is essentially the standard for any rails application. Therefore, it ranks high on my list of things I should have down solid on my machine.

** This assumes you already have `asdf` installed. See [asdf install instructions](https://asdf-vm.com/#/core-manage-asdf?id=install) to install via method of choice. **

# asdf
## So it begins...
```bash
brew uninstall postgis
brew uninstall postgres

# This takes some time to install compared to brew
asdf install postgres latest

# set as default postgres version
echo 'postgres 13.0' >>~/.tool-versions

# install different postgres version
asdf install postgres 12.4

# set `postgres 12.4` in .tool-versions of repository using postgres 12.4
echo 'postgres 12.4' >>~/your_repo/.tool-versions
```

## Takeaways
- Slight change in behavior compared to `brew`.
With `brew`, postgres was always running and it always "just worked". Truthfully, I never thought about postgres running. I would start a local server or open [postico](https://eggerapps.at/postico/) and everything always worked. Now, I am far more aware of things as I must explicitly start and stop postgres because I now specify which version is needed. This was a similar switching when using asdf for ruby. Before, I was inconsistent about running `bundle exec` before commands. With `rvm`, I would run into "it worked on my machine"..due to not realizing my machine had multiple versions of a gem installed and the latest one was being used. With `asdf`, if there is more than one version of anything, you are forced to `bundle exec`. This was a great habit to get into and also made me more aware of exactly what packages were being used. The difference with `postgres`, is there is not really an error. It is more like trying to use postico and not being able to connect because well...that version is not running. Same thing for applications, try to enter a rails console, I need to make sure to start postgres and stop postgres when I need to switch.

- The main takeaway is to remember to stop server first when switching!
For example, although `asdf current` said using verison 13, 12.4 was still running. When running `rails db:create`, the database was created under version 12.4 and not 13. I Had to stop 12.4, then start 13, then create, then all is good. It would be nice if `asdf` had a better way to handle this.

**The commands are**:
```bash
pg_ctl start
pg_ctl stop
```

**Query for Postgres Version**:
```sql
select version()
```

## Troubleshooting:
```
PG::ConnectionBad: could not connect to server: No such file or directory
  Is the server running locally and accepting
  connections on Unix domain socket "/var/pgsql_socket/.s.PGSQL.5432"?
```
- Try `echo 'export PGHOST=localhost' >>~/.bash_profile`
  - I only had this issue on one machine. Not sure why I did not need to do this on the second machine.


### Respect
- To [Jay Dorsey](https://github.com/jaydorsey) for introducing me to `asdf`.
- To [markdown-to-medium](https://github.com/yoshuawuyts/markdown-to-medium). I was surprised Medium does not support markdown! Kudos to Open Source for solving!
