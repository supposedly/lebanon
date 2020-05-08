# [leb](https://lebnxyz.github.io/lebmap)
An interactive map charting dialectal features of Lebanese Arabic. Based on the results of
[this survey](https://forms.gle/U4h1xtSJZ5nnv5Ku7) posted to /r/Lebanon on 8/13/19.

## How to use
The default tab, *Questions*, contains a list of everything that was asked on the survey. If you click on any one question,
you'll be provided with a list of the different words or situations that were asked about as a part of that
question, if any; clicking on one of these will finally show you a list of specific options for the realization of those
words or situations. This will activate the chart at the top to show how many people answered one way or another. Clicking
on one of these specific options will highlight pins on the map.

The pins represent respondents to the survey, where a bigger pin means more respondents at that location.
Clicking on a pin selects it, and clicking (& dragging, if you wish) on regions of the map selects all of the pins inside.
If you select a pin, several pins, or a bunch of regions, the only survey answers taken into account will be the ones from
those specific areas. You can use this to your advantage to compare how many people in different regions picked a particular
answer to a question.

The *Answers* tab currently does nothing, although you can at least use it to check the names of the regions you've selected.

The *Query* tab is a little bit more advanced, but also more powerful. It lets you search for specific combinations of answers
to see how many people in your selected region of the map chose the answers you're looking for. For example, let's say our survey
looks like this:

```
Question #0: How often do you drink coffee?
    Example #0: Espresso
        Option #0: Every day
        Option #1: Semi-weekly
        Option #2: Never
    Example #1: Decaf
        Option #0: Every day
        Option #1: Semi-weekly
        Option #2: Never

Question #1: Do you go to bed early or late?
    Option #0: Early
    Option #1: Late
    Option #2: I don't sleep.
```

Let's try using the code `question number . example number : option number` to refer to specific answer choices. The code `0.0`
would refer to "Espresso" (question #0 & example #0), `0.1` would refer to "Decaf" (question #0 & example #1), and `0.0:1` would
refer to semi-weekly espresso-drinkers (question #0, example #0, option #1). Similarly, `0.1:2` would refer to non-drinkers of decaf
(question #0, example #1, option #2). Notice that question #1 has no examples listed, and it instead jumps straight into the options;
that means our code for referring to it doesn't have to have the dot in it. `1.0` refers to early sleepers (question #1, option #0),
and `1:2` refers to our insomniac friends who don't sleep (question #1, option #2).

So! Under the *Query* tab, you can make use of these codes along with some operators to phrase a specific question about the survey's
respondents. If I want to find out about the number of people who drink espresso daily **and** don't sleep, I could phrase that question
as `0.0:0 & 1:2`; we've discussed those two codes, and the operator `&` says that we're looking for people for whom both the first one
and the second one hold true. If I want to find out about people who drink **either** espresso **or** decaf a few times a week, I could
phrase that question as `0.0:1 | 0.1:1`; the operator `|` means that we want either of the two to be true. Now, if I want to find out
about the amount of those people that can't go to sleep at night, I could use the query `(0.0:1 | 0.1:1) & 1:2`.

For convenience, you can use the list of questions below the textbox to have the question codes inputted automatically, and
you can add operators using the buttons below the textbox. The full syntax spec for querying is as follows:
- `option = option`: Find respondents who picked either both of the two options or neither.
- `option & option`: Find respondents who picked both options.
- `option | option`: Find respondents who picked either one option or the other.
- `!option`: Find respondents who did not pick this option.
- `(some operation goes here)`: Parentheses are used for grouping, like in PEMDAS. There's no default order of precedence other than
  left-to-right, so parentheses can be necessary.

## Comments
I finished the important functionality on 5/7/2020, which makes 9 months from start to end going by calendar dates, but
git history reveals that I actually only made commits on 24 separate days during that time. So that's procrastination
for you.

I'm kind of miffed that I published the survey before I actually knew enough about Lebanese to ask half of these questions.
A lot of them are based on shaky understandings of phonology or regional morphological variation and could have yielded
some way-more-useful insights had they been asked properly. Oh well. This project is a precursor to and building-block for
the remainder of @lebnxyz, which will include a comprehensive online dictionary hooked into a (hopefully better) survey that
will feed into a map like this one.

Also! Some of the code is hot garbage, especially the parts relating to the query functionality. Pardon.

## Build info
I really wanted to love [Parcel](https://parceljs.org), but (as of release 1.12.3) it just isn't mature enough yet to be reliable.
The issues at [parcel-bundler#501](https://github.com/parcel-bundler/parcel/issues/501),
[parcel-bundler#1294](https://github.com/parcel-bundler/parcel/issues/1294), and
[parcel-bundler#2262](https://github.com/parcel-bundler/parcel/issues/2262) slowly added to my mounting frustration, with the
third being the absolute last straw, so I switched to Webpack... and everything about the build process is a little bit ickier
as a result, but I'll gladly take icky over broken.

So if you'd like to tinker with the project yourself, the following four NPM scripts are relevant:

- `dev`: Serves a development build. Automatically opens in default browser.
- `prodserve`: Ditto, but serves a production build (just in case).
- `build`: Builds for production.
- `devbuild`: Bulids for development (just in case).

(And don't look at the remaining scripts. Please. It's for your own good.)

Of note is my *patent* abuse of Pages' "build from `docs/`" feature, allowing for local debugging from the project
root without requiring maintenance of a separate `gh-pages` branch to deploy from. If it works...!
