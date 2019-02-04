---
layout: post_default
title:  "Learning by observing"
date:   2016-10-20 15:48:00 +4:00:00
categories: ["blog", "programming"]
author: "alicja"
---
Recently I’ve had the pleasure of seeing [Sumana Harihareswara](https://www.harihareswara.net/) live documenting [a coding adventure](https://zulip.tabbott.net/#narrow/stream/test.20suites/topic/watch.20over.20my.20shoulder.20as.20I.20improve.20the.20linter) on Zulip. As I wanted to learn even more from this experience, I’ve reread it and tried to break it into high level activities.

#### Disclaimer
The following text is a rephrasing of a live-coding session. If you find any smart, hilarious and generally great quotes - those are Sumana's words when not directly attributed and Steve's or Tim's words for the appropriate dramatis persona.

## Updating the Linter

### an Adventure in Eight and Some Acts

#### Dramatis personae
The Developer - Sumana Harihareswara

The Counselor - Steve Howell

The Backstage Voice - Tim Abbott

### Acts[-1] - Finding an Adventure

The Developer noticed a disturbing string in one of the [recent commits to Zulip](https://github.com/zulip/zulip/commit/ca1789892dca27f8acdb57344f12d571ee5649ca).
An existing linter rule for assuring proper capitalization of `JavaScript` seems not to be working. It should find any unwanted occurrences of `javascript` or `Javascript` during testing and suggest a change in code.

### *What’s a linter?*

[source: Wikipedia](https://en.wikipedia.org/wiki/Lint_(software))

A linter (or lint) is a tool that flags suspicious usage in any computer language. Lint-like tools generally perform static analysis of source code. Linters are used for finding common errors in code, e.g. syntactic discrepancies in the code - mainly code that doesn’t correspond to style guidelines or doesn’t follow specific rules.

*I’ve had an example of that in my own code - Zulip linter tests were failing, because I didn’t use the 4-space indentation in JavaScript, as defined in the style guideline.*


### Acts[0] - Gearing up for the Adventure

Before the adventure, the Developer updates her local repository with a shell script, as to assure it’s up to date and any possible bugs are not caused by outdated code.

    #!/bin/bash

    # update a bunch of Zulip git repositories

    checkout_repo(){
        git checkout master
        git pull upstream master
        git push origin master
        git checkout -
    }
    for repo in zulip zulip-android zulip-electron zulip.github.io
    do
        pushd ${repo}
        checkout_repo
        popd
    done

### Acts[1] - In Search of Code

The Developer remembers she was facing a similar problem earlier and so she uses a `git log --grep="JavaScript"` command to find an appropriate commit, with which she finds:

    commit 2338421c6ddf8c520f2ebbb5b3c040aebc1ad197
    Author: Tim Abbott <tabbott@mit.edu>
    Date:   Tue Jul 12 19:22:21 2016 -0700

        lint: Add documentation lint check for JavaScript spelling.

She then copies the commit hash and runs `git show 2338421c6ddf8c520f2ebbb5b3c040aebc1ad197`, to see changes introduced in that commit. She has identified a change to `tools/lint-all` that has a pattern having to do with the word `JavaScript`:

    -markdown_rules = markdown_whitespace_rules
    +markdown_rules = markdown_whitespace_rules + [
    +    {'pattern': ' [jJ]avascript',
    +     'exclude': set(['README.dev.md']), # temporary exclusion to avoid merge conflicts
    +     'description': "javascript should be spelled JavaScript"},
    +]

She realizes that the solution is going to involve a regular expression ("regex").

To make sure that the appropriate file to edit is `tools/lint-all`, the Developer turns to the [testing documentation](https://zulip.readthedocs.io/en/latest/testing.html). It has a link to the [linter(s)](https://zulip.readthedocs.io/en/latest/linters.html) and she finds reassurance in the quote:

>Most of our lint checks get performed by `./tools/lint-all`

The Developer finds the relevant code on line `333`:

    markdown_rules = markdown_whitespace_rules + [
        {'pattern': ' [jJ]avascript',
         'exclude': set(['README.dev.md']), # temporary exclusion to avoid merge conflicts
         'description': "javascript should be spelled JavaScript"},

and she wonders if she should remove the temporary exclusion of `README.dev.md` - is it a thing of the past?

In her terminal she goes to `docs`, starts doing `ls RE` and hits tab to see what completes - there's only `README.md`. But she wants to be sure:

    $ find . -iname "*.dev.md"
    $ find . -iname "README.md"
    ./README.md
    $ find . -iname "*README.md"
    ./README.md

and it means there are *no* results when she searches case-insensitively for the files that end with `.dev.md`. Just to be sure that her syntax was right, the Developer searched for something that she knew was there, and it worked.

### Acts[2] - Branching

The Developer decides it's time to make a change, which means it's time to switch to a new branch in git!

    $ git checkout -b JavaScript-linter
    Switched to a new branch 'JavaScript-linter'

She quickly deletes the exclusion line and saves the file.

Now there is time for the real adventure to begin - why is the current rule, that searches for `[jJ]avascript`, not working with the commit in question (`ca1789892dca27f8acdb57344f12d571ee5649ca`)? That regular expression should catch the lower-case `s`.

Ah but wait! The actual line says:

    'pattern': ' [jJ]avascript'

There's a space before the bracket! Maybe - yes, the commit that just came through has the following:

    <a href="https://github.com/zulip/zulip-js">Javascript library</a>

Which means there's no space before the initial J! So maybe deleting the space will allow the linter to catch this problem.

The Developer decides to do just that and run the linter - she deletes the space and saves the file.

### Acts[3] - Testing the Null Hypothesis
The Developer wants to run the linter. She wants to save some time and decides not to run Vagrant - in a bold move she just starts up her `virtualenv` that has the right packages installed:

    $ workon zulip
    $ cd ..
    $ ./tools/lint-all
    ImportError: No module named typing
    You need to run the Zulip linters inside a Zulip dev environment.
    If you are using Vagrant, you can `vagrant ssh` to enter the Vagrant guest.

As this approach fails her, she decides to reprovision Vagrant:

    $ vagrant up
    $ vagrant ssh
    $ cd /srv/zulip
    $ python tools/provision.py

*The Developer goes in search for refreshments and conquers her email, as reprovisioning takes some time*

After the reprovisioning is done, the Developer runs the linter:

    (zulip-venv)vagrant@vagrant-base-trusty-amd64:/srv/zulip$ ./tools/lint-all

And she encounters a different result:

    javascript should be spelled JavaScript at docs/code-style.md line 140:
    [[1]](http://stackoverflow.com/questions/500504/javascript-for-in-with-arrays),

    javascript should be spelled JavaScript at docs/code-style.md line 141:
    [[2]](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml#for-in_loop),

The Developer quickly realizes the space in the regex was there to avoid the situation when a lowercased `javascript` in a URL would cause a linter problem. Additionally, the file because of which the adventure began (`templates/zerver/api.html`) is not causing the linter to complain, which may mean it's a word boundary issue.

Time to use new battle gear - the Developer decides to fire up a regex tool [RegExr](http://regexr.com/)!

### Acts[4] - In the Land of Regex

RegExr makes it easy to say *help me write a regular expression that catches THIS case but not THAT case*. The Developer copies and pastes the lines the linter is complaining about *now* and the line from `templates/zerver/api.html` that she wants the regex to complain about, into RegExr's web interface:

    <p>We have a <a href="/api/endpoints">well-documented API</a> that allows you to build custom integrations, in addition to our <a href="/integrations">existing integrations</a>. For ease-of-use, we've created a Python module that you can drop in to a project to start interacting with our API. There is also a <a href="https://github.com/zulip/zulip-js">Javascript library</a> that can be used either in the browser or in Node.js.</p>
    [[1]](http://stackoverflow.com/questions/500504/javascript-for-in-with-arrays),
    [[2]](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml#for-in_loop)

<a href="{{ site.cdn_path }}/other/regexr.png" data-lightbox="regexr" data-title="{{ img[1].desc }}">
  <img src="{{ site.cdn_path }}/other/regexr.png" />
</a>

She is going to try to find a regex that only matches what she wants to match and chooses the current `'[jJ]avascript'` rule as her starting point. Before she starts the regex struggle, she skims the code of `lint-all`, to make sure no comments require anything particularly special in the syntax of regular expressions. She notices a few things:

- the line she's editing is within the `markdown_rules` definition, which is within the `build_custom_checkers` function

- later, in another function (`check_custom_checks_nonpy` within the `build_custom_checkers` function), there's:

            for fn in by_lang['md']:
                if custom_check_file(fn, markdown_rules):

  So `build_custom_checkers` is a higher-order function that has other functions within it. But the main thing is, the rule she's editing is one that only gets applied to Markdown files (filenames ending in `.md`).

- the top of the file includes `import re` which is the part of the standard library that does regular expression parsing - she thinks that implies she can use regular regexes and there's nothing special about the syntax the linter will use

The view of the numerous rules fills her heart with joy -  it's great that these rules save us from bugs!

The Developer turns back to RegExr and she is confused - the initial rule `[jJ]avascript` matches everything, so why doesn't the linter catch the first issue? She remembers that it is in an HTML page, not a Markdown page!

In conclusion, she now has two problems:

- how to deal with the possibility that the word might have or not have a space in front of it (but exclude URLs)

- how to get this check to happen in HTML pages as well as Markdown pages

She wonders if she could build on others' work to say "if this is in a URL then don't bother with it"? She looks in the existing `lint-all` code and searches for "url", "http" with no luck.

There's a common saying in software, the Developer believes initially spoken by Jamie Zawinski:
> You had a problem. You decided to use regular expressions. Now you have two problems.

Regexes are legendarily headache-inducing.

Facing this adventure, she can't think of a better solution. She thinks of turning to the world-renowned Stack Overflow, a source of answers to numerous questions bothering developers, but she first decides to look through RegExr - they may have a library of solutions to common problems like this. She finds the "Community" section and finds [a rule that matches URLs](http://regexr.com/2rj36):

    /[-a-zA-Z0-9@:%_\+.~#?&//=]{2,256}\.[a-z]{2,4}\b(\/[-a-zA-Z0-9@:%_\+.~#?&//=]*)?/gi

which seems like a very long and complicated regex. Does it really need to be that complicated? Won't URLs start with HTTP? She realizes it's not always true, as sometimes there will be hyperlinks to references elsewhere in the documentation.

She checks if there are currently any docs that have `javascript` in their filenames:

    $ find . -iname "*javascript*"
    ./.git/logs/refs/heads/JavaScript-linter
    ./.git/refs/heads/JavaScript-linter

and she finds none, but that's not to say they will never exist.

She also realizes this RegExr example is clearly also matching email addresses. And there may come a day when in code there will be an address similar to javascript@facebook.com.

### Acts[5] - Enter the Counselor

Facing so many doubts, she decides to trust that other engineers will review her pull request and tell her whether there was a better way to do this. The best way she now sees fit to face the problem is to use the regex to exclude URLs, while also including HTML as well as MD files when the linter runs this rule.

*The Counselor*: For the proximal problem I'd consider just exempting any case of `/javascript` and avoid the nasty URL regex. (But now that you're looking at the URL regex, maybe make a note of that as something we can additionally clean up. If a simpler URL regex suffices for us, the code will be easier on the eyes and maybe it will run a bit faster too.)

The Developer fiddles with this regex in RegExr to combine it with the JavaScript check. She comes to a fork on the road - in the alternate universe where she got no advice from the Counselor, she would simply use the RegExr interface to figure out how to say *match `javascript` and `Javascript` **except** within URLs*. But no! She is in this universe, so she follows the Counselor's suggestion and just excludes `/javascript` for now.

*The Counselor*: I think it's a valid strategy to say we make the regex very broad and then exempt only `/javascript` cases for now. This mostly errors on the side of fixing things. There's some risk that somebody will have another legitimate use of wrongly-cased javascript someday like `http://learn-javascript.com`, but we can deal with that reactively.

The Developer goes to RegExr to learn how to do the exclusion she wants to do. She does a LOT of iterations while looking at the "Cheatsheet" in the lefthand sidebar of RegExr, and comes up with:

    [^\/-]([jJ]avascript)

Which catches the bit of that HTML page she wants - `zulip-js">Javascript` - but none of:

    [[1]](http://stackoverflow.com/questions/500504/javascript-for-in-with-arrays),
    [[2]](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml#for-in_loop),
    http://my-javascript-guide.com

(she just came up with that last one because the preceding hyphen strikes her as a plausible potential case)

The Developer is now going to figure out how to get this check into the HTML rule(s) as well. Maybe there's some way to abstract things, so it's defined in one place and run both by the HTML checker and the MD checker.

The structure of the linter seems to be that developers set up rules/rulesets, and then reuse them elsewhere. The Developer decides to define this rule within the ruleset of *capitalization of proper nouns in prose rules*. She sets up a new ruleset, just before the current definition of `html_rules`, and simply copies the formatting of the other rules.

To make sure she is right about how this structure works, she made a change and commits it. This will run the linter, to see whether it breaks. She saves the file and runs `git diff` in the terminal:

    +    prose_style_rules = [
    +        {'pattern': '[^\/-]([jJ]avascript)',
    +         'description': "javascript should be spelled JavaScript"},
    +    ] # type: RuleList
         html_rules = whitespace_rules + [
             {'pattern': 'placeholder="[^{]',
              'description': "`placeholder` value should be translatable."},
    @@ -327,11 +331,7 @@ def build_custom_checkers(by_lang):
              'description': "`placeholder` value should be translatable."},
             ] # type: RuleList
         json_rules = [] # type: RuleList # just fix newlines at ends of files
    -    markdown_rules = markdown_whitespace_rules + [
    -        {'pattern': ' [jJ]avascript',
    -         'exclude': set(['README.dev.md']), # temporary exclusion to avoid merg
    -         'description': "javascript should be spelled JavaScript"},
    -    ] # type: RuleList
    +    markdown_rules = markdown_whitespace_rules + prose_style_rules

The Developer decides to test one thing at a time - - change the rule *or* the structure of the rule-calling. She goes for the rule-calling structure and changes the pattern for `prose_style_rules` to `'pattern': '[jJ]avascript',`. Saves. Runs:

    $ ./tools/lint-all
    javascript should be spelled JavaScript at docs/code-style.md line 140:
    [[1]](http://stackoverflow.com/questions/500504/javascript-for-in-with-arrays),

    javascript should be spelled JavaScript at docs/code-style.md line 141:
    [[2]](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml#for-in_loop),

At first surprised, she quickly realizes she took out the space from the pattern - she adds the space in and runs again - it works!

The Developer is now going to try changing the regex. She changes it to `'pattern': '[^\/-]([jJ]avascript)'` and runs the linter, which seems to work.

Next she's going to try `test-all` which is more formidable and which she probably should have run earlier in this set of iterative changes.

*The Developer checks email and Zulip traffic, while waiting for the tests to finish*

*The Counselor*: Although not related to your fix, take a look at `json_rules` in your diff. :)

While she's waiting for that to run - tests seem to be passing - she wants to see what she would do to add... FAILED. The failing tests are Casper timeouts that some other developers are also getting, as they have mentioned reporting on their adventures. The Developer decides to ignore them.

She does a fresh diff and notices `json_rules` is completely empty, which may require further investigation in the future, to see whether it's worth blowing away or filling.

*The Counselor*: For the json thing, my two cents is that we should blow it away now. It's clearly empty and just clutter at this point, and it wouldn't be hard to restore the json section in the event that we come up with new json rules in the future.

*The Developer notes that and focuses on the regex for the time being.*

### Acts[6] - The Battle of Debugging

The Developer's current hypothesis is that the new regex works, and the new placement works, and the next step is to get the HTML checker to use it.

    html_rules = whitespace_rules + prose_style_rules + [

But wait! There are some failures! Some of these seem to imply a need to fix prose, some imply a need to fix the rule.

    $ ./tools/lint-all
    javascript should be spelled JavaScript at templates/zerver/api.html line 8:
        <p>We have a <a href="/api/endpoints">well-documented API</a> that allows you to build custom integrations, in addition to our <a href="/integrations">existing integrations</a>. For ease-of-use, we've created a Python module that you can drop in to a project to start interacting with our API. There is also a <a href="https://github.com/zulip/zulip-js">Javascript library</a> that can be used either in the browser or in Node.js.</p>

    javascript should be spelled JavaScript at templates/zerver/api.html line 29:
        <h4>Javascript</h4>

    javascript should be spelled JavaScript at templates/zerver/api.html line 39:
          <li><a href="#javascript" data-toggle="tab">Javascript</a></li>

    javascript should be spelled JavaScript at templates/zerver/api.html line 136:
    <div class="tab-pane" id="javascript">

    javascript should be spelled JavaScript at templates/zerver/index.html line 54:
        <p>{ trans }If this message does not go away, please wait a couple seconds and <a href="javascript:location.reload(true)">reload</a> the page.{ endtrans }</p>

There are five incongruities:

1. Is the one the Developer wanted to find, yay!

2. Is similar to 1

3. The hash should be excluded, but there is also some English to fix

4. Is to be excluded (id in an HTML tag)

5. Is to be excluded (href in an HTML tag)

*The Counselor*: Nice work!

She fixes the English bits and starts to wonder how to include a hash. She believes it needs special escaping, because she tried just including it using `'[^\/-#]([jJ]avascript)'` or `'[^\/-\#]([jJ]avascript)'` - running `lint-all` gave a lot of failures. She turns to RegExr, using her new knowledge gained from the five linter failures.

She finds some help in the "Community" examples - looking for the word hash it turns out that `\#` does indeed search for a hash. She tries to copy and paste the regex from her editor to RegExr, which responds with some errors about the ordering. She then switches the order to put the hash before the hyphen:

    [^\/\#-]([jJ]avascript)

And now it matches what she wants it to match! Also, it catches the quotation marks which she thought she'd have to manually check for.

*The Counselor*: haha, do you know what it's doing there? [*meaning the ordering error*]

*The Developer notes that she has a guess about it - something about a range of characters in ASCII - but she is going to try running it to see whether now it catches anything she doesn't want it to*

    $ ./tools/lint-all

Hmm. Wait. She realizes she does **not** want to catch the quotation marks. She adds `\"` to that set.

Adding it to the end causes `lint-all` to go back into *everything is failing* mode.

*The Counselor*: This is a good case to step back. You got a cryptic red flag on the hyphen, and you made a change that you don't completely understand, although it sounds like you have a hypothesis. What function does "-" serve in regexes?

The Developer thinks out loud:

*`-` gives you a range*

*so if it's at the start of a sequence....*

*or rather, in the middle....*

*it'll cause the regex to think "you mean everything between [char to the left] and [char to the right]"*

*The Counselor*: you probably want to escape that dash for future readability *alone*

And it turns out that is not the only reason! The Developer is now escaping it:

    'pattern': '[^\/\#\-\"]([jJ]avascript)'

And, since she's fixed the prose problems, `lint-all` gives her zero results. Yay!

### Acts[7] - Creating a Pull Request

It's time for the Developer to add a code comment explaining the cryptic regex:

    {'pattern': '[^\/\#\-\"]([jJ]avascript)', # exclude usage in hrefs/divs

She wants to have a fine-grained control over what she's adding in this commit, so she types outside of Vagrant (unsure if she should maybe do it in Vagrant):

    $ git add -p

*The Counselor*: You should always do git stuff outside of Vagrant, unless you just enjoy pain. :)

She reviews all the changes:

- she made prose fixes

- she changed the rule for finding the incorrect spellings of `JavaScript` and moved it to a new ruleset

- she removed the temporary `README.dev.md` exclusion, since that file doesn't exist anymore

The Developer makes the judgment call that all this is one logical change and decides to put it in one commit.

    $ git commit
    Running lint-all using vagrant...
    [sudo] password for sumanah:

Which pops up her editor to write a commit message. The Backstage Voice's commit message from July was `lint: Add documentation lint check for JavaScript spelling.` - she can't go wrong by copying its format.

*The Counselor*: This is a pretty borderline call, but I'll make the argument that you should actually split the commit--doc changes and lint changes.

The Developer argues that The Backstage Voice did a similar fix all in one go (prose + linter), so she's ok with doing the same right now.

*The Counselor*: If there was some subtle bug in the regex (not likely at this point, but just suppose the possibility), then it would be nice to roll back the lint changes without regressing the docs. But one commit is realistically fine here.

The Developer agrees she's not not necessarily following the absolute most pristine best practices here.

*The Counselor*: There are occasions where we need to combine the commits because without the lint change, the code fixups will actually break. But sometimes we're just lazy, too, haha. :)

The Developer made a commit and is now making the change to remove the empty JSON ruleset, to add in a different commit. She thought she could just snip out that one line, but then when she tried to commit, the linter complained, because guess what, the linter actually calls `json_rules` elsewhere!

When she looks for `json` in this file she sees a lot of things she is not going to change during this adventure. She is going to file an issue for someone else to look at it. She gets back to the state she wanted to be in, where she has not touched the JSON-related code.

    $ git reset HEAD

It is now the time for a pull request with a single commit.

    $ git status

It shows a lot of untracked files, but the Developer knows that the modifications she wants to push are in the tracked ones.

    $ git push origin JavaScript-linter
    Counting objects: 54, done.
    Delta compression using up to 4 threads.
    Compressing objects: 100% (7/7), done.
    Writing objects: 100% (7/7), 944 bytes | 0 bytes/s, done.
    Total 7 (delta 6), reused 0 (delta 0)
    remote: Resolving deltas: 100% (6/6), completed with 6 local objects.
    To git@github.com:brainwane/zulip.git
     * [new branch]      JavaScript-linter -> JavaScript-linter

She opens a new tab in her browser and GitHub agrees she has just pushed a new branch. She wants to start her pull request, but suddenly realizes she forgot to mention in her commit message why she was removing that exclusion for the nonexistent page.

The Developer goes back to fix it:

    $ git commit --amend

After updating the commit message she runs:

    $ git push -f origin JavaScript-linter

and reloads GitHub. Control-R on the pull request doesn't quite work - the new commit message shows up as the page is loading, but when it finishes loading, it has the old one. Cache issue? She goes to the `zulip/zulip` GitHub repo and then to `brainwane/zulip` and she tries clicking "Pull requests" and manually constructing one from her branch, and now the [pull request](https://github.com/zulip/zulip/pull/2029) is there.

The Developer waits for Travis CI to decide if it's going to complain when running the test suite.

### Acts[8] - Aftermath

The Developer is surprised how much time it took her to fix the linter issue.

*The Counselor*: Even seemingly small changes like this can be time-consuming when you're thorough. :) The good news is that the fix is broader than the original problem--we're looking not just for markdown, but also HTML, and we removed some obsolete temporary shim code. Also, the new code is cleaner is terms of separating out the rules. (Also, regexes are always tricky and time-consuming.)

*Travis CI concludes the Developer's code failed the tests.*

*The Counselor*: @The Backstage Voice the pull request seems to be breaking due to a provisioning thing unrelated to the Developer's fix.

The Developer remembers to create an issue for the `json_rule`. She runs `git blame` to see who added the `json_rule` and sees it was last touched by The Counselor in 3c401571. She runs `git show 3c401571` and sees it was a refactor.

*The Counselor*: Yep, my change was just moving code around. I'm about to investigate where the rules were actually emptied out.

The Developer has filed [an issue](https://github.com/zulip/zulip/issues/2030) and asks the Counselor to teach her more about the investigative skill he intends to use. She is particularly interested in the specifics of how to find deleted code.

*The Counselor*: First, I want to get the latest copy of master, so I do this:

      $ git checkout master
      $ git fetch upstream
      $ git rebase upstream/master

*The Counselor*: I then do `git log -S json_rules` to find changes that might have touched `json_rules`, and I find this change: `e71d8bb4b6b1e90f4b6c0fb9ab93cd9929c9d87e`

    $ git show e71d8bb4b6b1e90f4b6c0fb9ab93cd9929c9d87e
    commit e71d8bb4b6b1e90f4b6c0fb9ab93cd9929c9d87e
    Author: Tim Abbott <tabbott@mit.edu>
    Date:   Thu Apr 14 10:48:30 2016 -0700

        lint-all: Require newlines at end of JSON files.

*The Counselor*: So, this is a bit messy, but it turns out that when you call `custom_check_file` on a file, even if the `rules` parameter is an empty list, it still does a check for trailing newlines. So the current code's behavior is correct, although there might be a way to refactor the code to improve clarity. Checking for whitespace issues is decidedly non-custom, so the name of the function is misleading to the extent that it does non-custom checks.

*The Counselor*: It's a little bit of a performance hack that we try to sweep a file all in one pass, both for custom regex things and generic things. If we separated out those concerns, which would be cleaner for somebody reading the code, then we'd introduce a small performance penalty in that we'd have to open every file twice.

*The Counselor*: We could go down a huge rabbit hole trying to clean this up, but I think for now the simple fix is to clarify the comment. Something like:

    # It is okay that json_rules is empty, because we will
    # still check JSON files for whitespace

*The Counselor*: The current comment is `# just fix newlines at ends of files`, which sort of implies that, but I think we can be more explicit.

*The Counselor*: It might be worth making that quick fix, so that as a side effect, your re-push will kick off a new Travis build. I'm curious if the Travis errors you got are permanent or just a one-time flakiness thing.

The Developer makes the quick fix for the newly filed issue in a new commit and re-pushes her code.

*The Counselor*: Indeed! Incidentally, your build passed, so I think the prior failure was just some sort of flakiness in the Travis infrastructure.

Phew!

*The Backstage Voice*: Yeah, if you ever see an error about being unable to download a URL on the public internet, that's almost certainly caused by internal networking infrastructure problems with Travis CI.

The Developer wonders if it's a durable enough prediction that it should go in our testing docs?

*The Backstage Voice*: I think it might be better to add some retry logic around the network-intensive parts of our testing system (e.g. `apt-get` commands). It's probably about as much work as writing good docs for this, and has the advantage that it might actually solve the problem.

The Developer files [another issue](https://github.com/zulip/zulip/issues/2034).

THE END

## Lessons Learned

1. The developer's journey requires a methodic and mindful approach. They have to first understand the problem and identify the probable causes. They should not be afraid of making mistakes and testing hypotheses.

2. Reading the documentation while looking for answers to specific questions seems to work well while trying to solve a problem. Initial familiarity with the documentation helps with finding the relevant documents.

3. Asking for help after trying and looking is the logical next step. There is no point in getting lost in a jungle of bugs.

4. Good understanding of what's exactly happening in code is crucial. Sometimes developers look over small discrepancies, but they may become causes of bigger problems.

5. Even seemingly small changes can be time-consuming when one is thorough. While solving one problem a developer may encounter many other problems on many levels.

6. It is often wise for a developer to be cautious about believing they know the truth. When approaching a problem it is important to have some history of the project that gives a sense of how risky the change is. It is also helpful to approach it with the scientific method and creating hypotheses about what could fail if they make a change.

7. Coding is an adventure. It's demanding, requires a lot of patience and perseverance, but it's a most rewarding endeavor.
