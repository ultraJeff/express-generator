# express-generator (and friends)

*NOTE* : This is ripped off from [the official Express generator](http://expressjs.com/starter/generator.html "<3")

## Introduction

It exists here only for didactic reasons insofar as **to show how easy and powerful the Express command line interface (CLI) really is** and how it can act as a **powerful, yet barebones server boilerplate that is Heroku ready**.

**SIDEBAR:** If you haven't messed around with Heroku yet, [do it](https://devcenter.heroku.com/articles/getting-started-with-nodejs#introduction). Here's why:
  - It lets you get that dumbass cat food delivery service app idea you've been mulling over for months up on the internet in literally 30 seconds.
  - It has built-in database support and (I think) it also provides a GUI. 
    - Heroku can easily integrate your awful app with its built-in Postgres database.
    - You can also use MongoDB (and other DBs via Add-Ons) as well.
  - It has a beautiful and simple GUI. Purple is slick.
  - It has **dataclips**
    - Essentially, dataclips are SQL queries that have been converted to URL endpoints for AJAX consumption. Hells to the yeah!
  - **It's free** while you are experimenting. (A viable product *will* cost you though)

## Procedure

**This repo is the final product**: the "spun-up" server with bonus front-end Jade boilerplate (sans a build system) **that Express created**. Here's how you do it yourself:

### Instal Express Generator
    npm install express-generator -g
  
### Create a Directory for Your New Express Server + Jade Boilerplate.
    mkdir your_awesome_app_name
  
### Get Expressive
    express
  
### Install Your Dependencies
    npm install
  
### Start That Shit
    npm start
  
### Navigate to [http://localhost:3000](http://localhost:3000)

### Done!
  You got yourself a backend that you can customize to your hear's content: Grunt or Gulp? MV*? AMD or CommonJS? Express DGAF. You can even swap out Jade, but I don't know why you would!

P.S. - I'll add some info about getting up and running on Heroku soon. In the mean time, just dl the [Heroku Toolbelt](https://toolbelt.heroku.com/) and figure it out. I believe in you!


##ProTip: New GitHub Repos from the Command Line

**Have you ever noticed how difficult it is to create a new GitHub repo from the command line?**

You got your cat food delivery service app set up with Git locally by running `git init`. Sweet! Uh-oh though... now you want to **push your new local Git repo to a brand new GitHub repo**. Yeah, you can go to GitHub.com and make a new one, but that sorta sucks after a while.

Long story short, **here's a BASH script that'll automate the process of creating a new GitHub repo from the command line** with instructions to get you up and running. (This super-cool code snippet is [based almost entirely on this guy's trick](http://viget.com/extend/create-a-github-repo-from-the-command-line, "CHYEAH!!!").)

###The Meat

####Add this to your .bash_profile

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

###The Potatoes

There's a little bit of config to do.

####Generate GitHub Token

You have to generate a GitHub personal access token for this to work. Go [do that](https://github.com/settings/tokens) and come back if you haven't already. Don't ask me what permissions you need - I don't know. *Rule of thumb:* select as few permissions as possible / that you think you can get away with.

*NOTE:* You only need one token for all things at this permission level: you can reuse the same token for multiple repos.

Got it? Great! Moving on.

####Set Global Git Vars

Add your GitHub username, token, and tokentype to your global `git config` like so...

`git config --global github.user <your_username_here>`

`git config --global github.token <your_token_here>`

`git config --global github.tokentype <your_tokentype_here> #It's either http or ssh`

Now you don't have to run these commands `--global`ly and you don't have to namespace the variables with `github.*` but it's useful! (Is it safe? ...Probably? Just don't somehow add your .gitconfig to your public repo.)

####Get Back in the Saddle

You got all the config done, now send shockwaves through your command line by issuing this line of code

    source ~/.bash_profile
  
AHHSOME! Now create a new directory folder as explained above in the **express-generator** section and once you're at the root, simply type
  `github-create`
  
Now go check out your GitHub page. If everything worked as it should have (thanks to my flawless and extremely international diction (TODO: rm slang -g), you should have the repo that you just created locally on GitHub! Plus, the remote URLs for your local Git repository are already set up. Next time you make a change that you're ready to push up, just type

    git push origin master
  
**Congratz you win the tutorial!** 

Drop me a line, or troll the shit out of me for positive feedback and angry correction demands.

-Jeffers
