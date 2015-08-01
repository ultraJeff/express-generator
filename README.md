# express-generator (and friends)

*NOTE* : This is ripped off from [the official Express generator](http://expressjs.com/starter/generator.html "<3")

## Introduction

It exists here only for didactic reasons to show how easy and powerful this CLI really is and how it can act as a great, low-cost server boilerplate that is Heroku ready.

(If you haven't messed around with Heroku yet, [do it](https://devcenter.heroku.com/articles/getting-started-with-nodejs#introduction) - it lets you get your dumbass idea up on the internet in like three lines of code. It's insane. *plus it has databases built in with dataclips - if you don't know what a dataclip is, Google it and get the Kleenex. You're about to have a braingasm*

## Procedure

This repo is the final product, the "spun-up" server + front-end boilerplate that Express created. **How do you get here?**

### Instal Express Generator
  npm install express-generator -g
  
### Create a Directory for Your New Express Server + Boilerplate
  mkdir express-generator //or w/e you want to call it
  
### Get Expressive
  express
  
###Install Your Dependencies
  npm install
  
##Start That Shit
  npm start
  
###Then navigate to [http://localhost:3000](http://localhost:3000)

**Dunzo. You got yourself a backend. Plus, it's already Jade ready. **

P.S. - I'll add some info about getting up and running on Heroku soon. In the mean time, just dl the [Heroku Toolbelt](https://toolbelt.heroku.com/) and figure it out. I believe in you!


##Pro Tip(s)

Have you ever noticed that it's not straight forward to create a new GitHub repo from the command line? Like, you got your whole boilerplate set up with Git by running 'git init' and now you want to push to a GitHub remote (because all the cool kids are doing it). Only problem is, wtf is the command? Well one does exist and it uses cURL but it's ugly and bulky. Here's a BASH script that'll automate the process with instructions to get you up and running. (Surprise, surprise, this sweet code snippet is also [stolen](http://viget.com/extend/create-a-github-repo-from-the-command-line, "CHYEAH!!!"). I did fill in the information gaps that tripped me up though.

###Add this to your .bash_profile

```bash
github-create() {
  repo_name=$1

  dir_name=`basename $(pwd)`
  invalid_credentials=0

  if [ "$repo_name" = "" ]; then
    echo "  Repo name (hit enter to use '$dir_name')?"
    read repo_name
  fi

  if [ "$repo_name" = "" ]; then
    repo_name=$dir_name
  fi

  username=`git config github.user`
  if [ "$username" = "" ]; then
    echo "  Could not find username, run 'git config --global github.user <username>'"
    invalid_credentials=1
  fi

  token=`git config github.token`
  if [ "$token" = "" ]; then
    echo "  Could not find token, run 'git config --global github.token <token>'"
    invalid_credentials=1
  fi

  type=`git config github.tokentype`
  if [ "$type" = "ssh" ]; then
    conn_string="git@github.com:$username/$repo_name.git"
  elif [ "$type" = "http" ]; then
    conn_string="https://github.com/$username/$repo_name.git"
  else
    echo "  Either token type was not enterred correctly or is empty.\n  It must be one of 'ssh' or 'http'.\n  Run git config --global github.tokentype <ssh|http>"
    invalid_credentials=1
  fi

  #Added more error info here since this return 1 is pretty effin useless during debug
  if [ "$invalid_credentials" -eq "1" ]; then
    echo " Failed:"
    echo $username:$token on $tokentype sucks!
    return 1
  fi

  echo -n "  Creating Github repository '$repo_name' ..."
  curl -u "$username:$token" https://api.github.com/user/repos -d '{"name":"'$repo_name'"}' > /dev/null 2>&1
  echo " done."

  echo -n "  Pushing local code to remote ..."
  git remote add origin $conn_string > /dev/null 2>&1
  git push -u origin master > /dev/null 2>&1
  echo " done."
}
```

###Generate GitHub Token

Of course now you have to generate a GitHub token. Go [do that](https://github.com/settings/tokens) and come back if you haven't already. Don't ask me what permissions you need, because idk; select as few as you think you can get away with. 

*NOTE:* You only need one token for all things at this, possibly arbitrary, permission level.

Got it? Great! Moving on.

###Set Global Git Vars

Add your GitHub username, token, and tokentype to your global Git config like so...

`git config --global github.user <your_username_here>`

`git config --global github.token <your_token_here>`

`git config --global github.tokentype <your_tokentype_here> #It's either http or ssh`

Now you don't have to run these commands `--global`ly and you don't have to namespace the variables with `github.*` but it's helpful! (Is it safe? ...I defer to another for this answer)

###Get Back in the Saddle

You got all the config done, now send shockwaves through your command line by issuing this

  source ~/.bash_profile
  
AHHSOME! Now create a new directory folder as explained above in the **express-generator** section and once you're at the root, simply type
  `github-create`
  
You're amazing and everyone loves you. Go celebrate by staring at something that isn't a screen. (Maybe just look. Staring isn't very polite.)

-Jeffers
