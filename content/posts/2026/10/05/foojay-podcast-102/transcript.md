**[0:00]** Foojay walked away from WordPress after six years and 2,000 articles. Today we ask why and whether a CMS is even the right way to run a content site.

**[0:11]** Welcome [singing and music] to the Foojay.

**[0:17]** Welcome back to the Foojay podcast. If you've been reading Foojay.io lately, you may have noticed something changed under the hood. The site quietly moved off WordPress and onto Huo, a static site generator. So there's no longer a CMS login, a database, or an admin panel. No, it's just Markdown and Oski Dog files built and deployed straight from GitHub. Today, we're digging into why that move happened and into the bigger question behind it. Is a traditional content management system still the right way to run a content site in 2026 or has static tooling caught up? We'll talk about the real security and cost numbers behind WordPress's plug-in ecosystem and put very different static site generators head-to-head. Huho and Jackal, the veterans versus Rock, the Java and Quarkusbased newcomer. To help unpack all of this, I'm joined by two guests, Andy and Holly. Welcome. Thank you for joining this recording. can you shortly introduce yourself, please?

**[1:22]** Sure. I'm Holly Cummins. I'm part of the Quarkus team. and I do all sorts on Quarkus. So, I think I I've talked to you before about sustainability and Quarkus' sustainability credentials. I do a lot with dev services as well and Quarkus testing. But most recently, and I can't even remember why I started doing it, [laughter] but most recently, I've been doing a lot of work with the Quarkus website.

**[1:46]** Okay. Indeed, we were in an earlier podcast. I cannot remember the number exactly, but I will add it to the show notes. So, if people want to know learn more about Quarkus itself, we have a different podcast about that and it's linked in the show notes. Okay, Andy, over to you. I'm Andy Deovin. I've been in the Quarkus team for a while now since before V1. It all started with code.quarkers.io io that I made and I did various thing on quarkus mostly around dev tools and then I switched to what I really love I love web stuff and I love front end

**[2:30]** So I try to combine my back end and front end skill because Quarkus is boss and open that door so that we can have a web stack on Quarkus. So, I I've been focusing on that and I it seems to work well and people are using it a lot and the last in the game is rock that we're going to talk about.

**[2:54]** Mhm. And you actually told me on LinkedIn about rock. So, I published some posts about the move from Hu from WordPress to Hu. you both sent some love because I did this move. the other hand you said why didn't you use rock and I have to be honest I considered rock I also considered gbake both Java based systems to build static websites but I knew VUO from other projects so that's where I went in that direction now you're working on a blog post for Foojay about what's happening with WordPress and static websites you prepared a lot of numbers already there's a lot of fuss about WordPress. What's what's going on? What's wrong with WordPress or what is happening? I don't think that WordPress is really the issue. The core of WordPress, I think, is pretty stable and work pretty well.

**[3:55]** If you look closely, it's mostly about the plugins that are there. And you need plugins with WordPress. It's how it's designed. Without plug-in, you can't do much. and they found more than 10,000 vulnerabilities in those plug-in since 2025. And now we are talking a lot about supply chain attacks. That's a trend war those days. And now you can get act on your WordPress website just with updates. If the plug-in updates then you can get act which is pretty bad. Yeah. [laughter] for people who are not familiar with the comparison between systems. So WordPress is a content management system. You log in, you have a database behind it. Then what is the main difference with the static website and the thing that for instance rock also does. First you need a server to run a CMS. and you can attack that CMS just because it has a server. If you're using static, it's totally different because your data will be synchronized with git. So it's version you have journalization divs you can see what's happened and you have all the history

**[5:17]** And g is meant to do that job by design and it's secured and it's his job. Then you generate from that content your website which is fully static. So it can't get act it's just static pages that are deployed. So that changed the world game I suppose

**[5:40]** And so there must be a problem that you cannot fix with the static website as these CMS systems still exist. So I found one of the things that I had a problem and a problem with when moving Foojay is the whole discussions so that people can react on a blog post because there is no database to start these reactions. But again I found a solution in GitHub discussions. So every post is linked to a GitHub discussion. So I found a way to fix this even within a static web. But I guess

**[6:15]** That's exactly what we do as well. It's a very easy solution and again it's free. [laughter] but I think like a web shop that's not the kind of stuff you built with rock or

**[6:29]** I would say all the things that you can do with plugins in WordPress can be achieved with external services and most of those are actually existing as external services and that's the way it's supposed to be. most of the content is static. So keep your content static, your use static website and then if you need to add stuff on to that web shop, payments users commands then just use services for that part. If you then go to rock which is the things both of you really love because it runs on Quarkus Holly you did the move from Quarkus io from Jackal to Rock. So it was already a static website correct? Yeah, it was already static, but it was showing its age.

**[7:20]** Was the website showing his age or is the Jackal system?

**[7:24]** The Jackal. So, was it was there was quite a coincidence in timing actually because you did the Foojay move at I think about the exact same time that we did the Quarkus.io move and both of our blogs ended up starting with almost this exact same line as first line as well of like we've done all this work and nothing's changed. But for us, it really was about the developer experience for us maintaining the website because

**[7:47]** It was such a nightmare for us to sort of try and navigate that Ruby ecosystem and you know just to stand up the website. You had to like download a container to try and get the Ruby gem to work, but then that container would stop working. So then you'd have to try and like get it onto your machine, but then your machine would have an old version of Ruby and then you'd be like half a day down and you hadn't actually written anything at all and you had still hadn't managed to stand up the website and then you just gave up and left the website unupdated.

**[8:14]** So it was not a good developer experience.

**[8:18]** Yeah. So you have these these different systems. So Jackal runs on Ruby, Huho runs on Go, Rocks on Java because and JB GBake also. is it because you use Java that it's easier to set up and to keep it running?

**[8:36]** I think that's part of it.

**[8:40]** But probably only a fairly small part of it. So, you know, we now in order to have sort of a live preview of the site, we just type Quarkus dev, which is a command that all of us already have on our machines. So that's one little bit of friction that goes away, but a lot of the rest of it is just about sort of, you know, being more modern, being easier to install, that kind of thing. At some point, I think we probably will

**[9:08]** Cuz we had like a few Ruby plugins that we used with Jekal, but we were kind of limited in what we could do with them just because we didn't want to spend that much time writing Ruby code. Now that it's Java code, I think we have that sort of unlocks more cool things that we can do with the website. Still static, but cool static things. So Rock is involved is using Java to build the website. Do I need to deploy it to a Java server or is Rock only used in the build time?

**[9:38]** It's only in the build time. It's it's pretty much the same as all of the static systems that you have your sort of content that goes in which would usually be like markdown or ask do or something like that and the job of the generator is to using a program which is written in Java or Go or JavaScript or whatever take that markdown and then convert it to HTML but then at the other end it's there's no there's no Java there there's no there's nothing except the HTML. Yeah, the seat seat is the same in the end whether you use ego jill or rock the output is supposed to be similar.

**[10:17]** That's also what I love about the new way of Foojay how it works. So an author who wants to contribute content they just make now a pull request they provide an a markdown file or an oskid doc file and then the whole build process runs on GitHub actions. I guess with rock the same thing can be done. It can also be executed on GitHub.

**[10:40]** Yeah, exactly that. And then we use surge for previews as well. So, you know, runs on GitHub and then the author gets a preview and so it's just a really nice flow.

**[10:51]** Okay, the preview thing that's something I need to remember because that's one of the first requests I got. [laughter] Where can I see a preview of my post that I just contributed? I don't have it yet. So, I should look into it. if I work with rock as a Java developer and I find something missing, is that something I can contribute in the Java codes? Because yeah, you say it's a Quarks application that's running. So, can I create an extension or a plugin or I want to format specific markdown into something which was not there yet. Is that something that I can add to the rock system?

**[11:31]** Completely. So you've got you've got sort of three three options and three levels of change. so it could be that actually you just need to make like a little tweak to what's already there in rock and so then you know the community is super welcoming and so you can just make a PR to rock. alternatively, you might want to do something bigger. And so then you can write what's called a Rock plugin, which is really just a Quarkus extension. And so then that you can either keep that to yourself or if you think it's useful to the community, you can then contribute it back to Rock or contribute it, you know, just have host it elsewhere. and another option that you have, which I wouldn't I don't really exactly recommend this because it's it's but it it's really useful to know it's there is that because it's all Java, it's so much more accessible for us to change. So like I'm doing work at the moment to convert the Quarkus workshops which are hosted separately and are just pure ASKI do. I'm converting them to rock.

**[12:31]** And so while I was doing that, I, you know, hit a thing where something didn't do what I needed it to do and there was, you know, a missing feature in Rock. and so now what I'm doing in the build for that as a temporary measure is I'm just hot patching rock. And he's looking horrified. [laughter] So again, you know, that's that's not probably what you want to be doing long term, but it is really useful to know that, you know, if you need to,

**[12:54]** You can, you know, you can just change it and rebuild it and patch it and run it.

**[12:58]** Mhm. what I did to move move from WordPress to Huho is I created a lot of Java Gbank files Java script files I should not Java not JavaScript but Java [laughter] script files to pull [clears throat] content from WordPress get it from the SQL database that was one thing the migration part for the migration do you have something ready WordPress to rock or is that a one-time job that people should manage in a different way.

**[13:30]** It's really funny because the article was written yesterday. Maybe that's a coincidence. I don't know. No, we actually published it a bit quick quicker than it should for this podcast. So, we have a tutorial on how to migrate from WordPress to rock and it seems that it's pretty straightforward. still we have an issue on making it more automated. Mhm. yeah, I actually did a lot of cleanup at the same time because yeah, in the WordPress system that we had for Foojay, people could contribute content with WordPress blocks or with OSI do or with Markdown. So we ended up with some strange mix of articles and now everything is back to clean markdown and OSI do if people want to do that. so I guess having access to your SQL database is probably the easiest way to get content out of it.

**[14:29]** So I haven't been writing the article and I haven't tested it yet. but I think it there is a tool NPM NPX tool that just extract your content and as Markdon

**[14:43]** With rock if we just I'm still considering if I should move Foojay to rock. So sorry if I ask some stupid questions. [laughter] one of the things that I do now is I have a schedule running every few hours that checks for instance a repository with all the jucks another repository with all the Java champions. then all the iical feeds of the jucks just to pull in content for the calendar and the jucks page. So these are again jibbang Java files which are executed on GitHub actions. Is this something I could add to rock and use rock and quarkus to run or is the gbank flow still a good flow even when using a rock system? You can choose both are viable. if you do with rock you will have to create an extension I suppose in order to do that but if you do it with GitHub action you will put that as data

**[15:45]** In a directory and I think that's the best the easiest way

**[15:51]** Yeah that's the pattern that we use in Quarkus IO is we've got all of these sync scripts which are just constantly refreshing data in a data folder and then that gets on from in the build everything in the data folder that then gets injected so that you can use it in your templates.

**[16:08]** Mhm. If [clears throat] all these systems use a bit the same approach, Jackal, Huho, Rock, why should people choose one of them? I chose Huo just because I knew it. and I also found out that cloth tools also know it very well. So, it has a long history. So, they have a long history of knowing how to do things and create templates and stuff like that. Is rock mature enough that AI tools also know it to help you build a website? so there are two question in your [laughter] in what you said. So the first question about rock versus jil versus ego. that's a very good question and I think it would be unfair to really put them in the same box.

**[16:57]** Rock is pretty new. Still I think there is something that can put it close to them and that thing is the fact that it's on top of Quarkus Quarkus is already doing 90% of the job that needs to be done and that makes a huge difference because if you look at Yugo or JKL most of the thing are coded as part of their core. So all the network all the everything is has to be maintained and created with rock. It's just a small box on top of Quarkus that does the final step to make it a static C generator and that make it pretty viable in long term since Quarkus is doing all the business that needs to be done to deal with network web JavaScript bundling all that is Quarkus and then you just have that small box that can make it a static generator. So that I think is a strong reason why rock is viable in the future.

**[18:01]** The second reason is that we are comparing here also rock to WordPress and that's pretty new. If you took if you take Jill or Yugo they were in their own domain and I don't think they were trying to compete with WordPress.

**[18:18]** And just before you were also saying why would you choose static over CMS? those goes together. I think the missing piece is the tooling. you the G git is adding quite some friction and that's true. If you don't have a web or someone that at least know a bit of web stuff, it's really hard to use a static generator with rock. We're really trying to close that gap to make it as small as possible. There are still fully honest there are still things that need to be done to make it as good as WordPress. WordPress is the best and that why it's used by thousand of people millions of people I would say I think 50% or 60% of the web is actually WordPress even higher at some point

**[19:08]** Where are we going right [laughter]

**[19:11]** And now if you link that fact plus the fact of the vulnerabilities [laughter] then it's bad

**[19:21]** What's going to happen so they need to move fast those guys [laughter] and switch to something sustainable and yeah so we're trying to close that gap to make the experience nearly as good with rock as it would be with WordPress we have an editor we have a block editor you were talking about a block editor just before and we integrated the g synchronization so that you can do the wall flow from your web UI I don't think that's enough

**[19:52]** Fully honest but that's a start, a good start and at least we can u say we we're trying to get there you know

**[20:02]** So you [clears throat] have this online editor so that's indeed a friction I already discovered for the new Foojay is some authors are not used to using GitHub or git flow so indeed most of the authors on Fujers or do something with Java code so they are used to it but then you have some people who are more into the marketing side of a project, they have some friction. So that's what you try to solve with Rock is that you still have this is what you see is what you get kind of editor. Yes, the big difference with WordPress is the fact that the editor is not hosted online at least for now. That's something that we can explore in the future if people ask for it. But for now you start your dev server locally and you have your editor as part of it and the editor is just abstracting that layer of complexity that you have to edit those markdown file commit them with git and all that is integrated in that editor and the editor has what you just said like what you see is what you get a really nice block editor close to what you are actually have in WordPress and nowadays

**[21:18]** People are used to sorry what that tool it's notion they like notion editors which is a block editor and we use a similar editor in rock

**[21:31]** So people will at least be will have something familiar

**[21:36]** Yeah I use obsidian which is a similar tool is you actually it works like a word kind of interface but it's actually markdown files behind it. So this is the standard file format for most of these things. So it actually makes sense that you have some kind of what you see is what you get editor. It's it's the whole it's the git flow thing that is blocking some some people of using this. and then the remaining question is if I use AI tools to build a new website. Oh yeah, that was

**[22:08]** And I ask it use rock will it already be able to understand what it needs to do what it needs to set up as a project and create an a start project for me

**[22:21]** Yeah when we did the conversion I used AI to create a converter because we couldn't convert in one go but when I was when I was using that it absolutely knew where it was going to it knew what it needed to look like on the other side.

**[22:38]** You you're challenging me to ask cloth now try to do Foojay with rock. [laughter]

**[22:46]** We've got we've got a ticket open for the Hugo converter, but we haven't [laughter] written it yet.

**[22:51]** Okay, maybe we can combine combine both of these. What I also found out is I published on the Foojay Slack to the authors. I'm working on this. This is the new version. It will soon come. First command. yeah, but these are markdown files. I prefer Askidoc. Ask do has some advantages. You can align columns and tables and do some other fancy stuff. It was quite easy to add to HUO. Claude helped me there again. so now people can also contribute in as doc format. Is that also something which is possible with rock?

**[23:27]** Yeah, it's it's got both. So it's got it supports markdown and it supports as do using Asky Dr. J under the covers. and it also supports ASI do using a new engine called upupic. I'm not sure how it's pronounced. It's Y u P I K. and that one is pure Java. I mean obviously Asky Dr. J is pure Java, but it just is Java pretending to be Ruby.

**[23:54]** And [laughter] the ask the upupic one is pure Java. it's not quite as capable as full ASKI do but it's the gap is closing. so we're hoping at some point we will switch over to the pure JavaSci doc. But you've got you've got both choices.

**[24:12]** So then we have the advantage of the very wide range of Java libraries which then can be used again. I used some of these library in my jbang scripts like guit and stuff like that to parse content. So that's the same kind of libraries that you benefit from with rock to convert from one format to other with this whole move. we went we stepped away from a hosted paid WordPress system. I was able to do the whole migration. I had a colleague with CSS expertise jump in to fix some of the things that I messed up and that cloth let's blame Claude on this [laughter] some accessibility things. so with a very small team I was able to re replicate something which was built over the years by a team. So there's both a cost reduction in WordPress, there is a cost reduction in the whole team which was needed to maintain WordPress and do updates every day sometimes. can I now run this kind of website completely for free? Is that also what you see with GitHub pages and GitLab pages and these systems?

**[25:33]** Yeah, I mean I still find it kind of amazing, [laughter] but yeah, you can do it absolutely cost free because so you so GitHub pages is so generous in terms of what they give you. So you can have GitHub pages. for our previews we use Surge and again we're on the free tier on search. and it's it's very generous. there's again I think with the discussions that's an external service but it's just using GitHub discussions. So it's free

**[26:03]** And also for the work did you like you migrated quarkus IO were you in did you still need a marketing team or SEO team or who was involved in that whole migration? So the migration was mostly me and also one of our docs team Ralph was sort of quite busy. Every time I would find a missing feature [laughter] he'd sort of jump in and fill gaps. I think it's probably a little bit different for us because we were on Jackal and then we moved to Rock. So it wasn't such a big shift as moving from a CMS to a static generator. so we found that everything was more or less the same as it was before except our developer experience was better.

**[26:52]** Mhm. We can also talk about a funny story. Funny, not that funny, but did you hear about Alps jug story the jug that is in the Alps?

**[27:03]** Oh yeah, you mentioned something in where you roasted me [laughter] that you actually were able to recover a website. Correct.

**[27:12]** Not exactly. So, they had 17 years of data in WordPress, all gone in one day.

**[27:22]** Ouch.

**[27:23]** Yeah, that's the original reason I moved away from WordPress for my personal blog. I think five years ago or even more because it got wiped again and again. I don't know which plug-in was causing it, but that's indeed something bad can happen. They were kind of lucky because they had partial data stored in the event service external service which wasn't act it seems. So they managed to fetch that data to build back JSON files and then they switched to rock I think he said in a few hours and the seat was back online with most data and even more beautiful than it was. So the experience I think was pretty good.

**[28:07]** Yeah, that that's maybe an overlooked issue is that yeah, everything from WordPress is in a database.

**[28:16]** I'm wondering you need backups. I'm wondering how many people with a WordPress website have a backup because yeah, you can rely on your hoster

**[28:26]** Who should take backups but

**[28:28]** And you need to pay for those backups, right? Yeah, and you need to pay for them or you need there are again plugins to make a backup of your database and get it. I think you can get every day a mail with your database, stuff like that. But if it's not in place and you have a security issue and someone is able to wipe your database, then everything is gone. The worst thing what happened with my website?

**[28:54]** It's when I suppose

**[28:55]** It's indeed when what happened with my website is my content was still there but it was infected with a lot of redirects to let's call them adult sites and stuff like that. So

**[29:07]** That's bad too.

**[29:09]** Yeah, that's not the kind of content you want on your personal blog. So it was a very easy decision to kill WordPress at that time. [laughter] so yeah, having your whole history now in a git project is I think the most safe way you can have at this.

**[29:29]** Yes, we just need tooling on top of that. If we had [clears throat] like you could have the exact same experience as WordPress hosted and everything but just backed by that system and you would have a really similar experience.

**[29:43]** Mhm. Still you have this horror stories of people telling yeah I did something wrong on kits and my account got suspended and I lost my whole project but even then you have your local history

**[29:57]** Normally.

**[29:57]** Yeah.

**[29:58]** Yeah. I think because it's distributed it means that you do have that point of failure on GitHub but then if that failure happens then you just go elsewhere. You just you've got your local copies, you've got your local history, you've got ref log, and so you can recover so much more easily and you can see what's going on as well because it's flat files rather than a data database. It's just so much more accessible.

**[30:20]** Mhm. I also have a fun story related to the move of Foojay. We had over 500 or 600 discussions on the WordPress website and I tried to import them as discussions in GitHub

**[30:33]** And I set up a new account because I was thinking this is probably a bad idea and I got that account got suspended after 20 posts in one minute. So, [laughter] so I decided to do it differently. So they are now as as static content again below the old posts and only the new discussions are on GitHub. So yeah, you still have to take a bit of care of what you do with your accounts. So has to

**[31:01]** There is another funny story. I don't know if you remember but I think it was three or four months ago that Entropic released their wall closed at least the client site part of the code.

**[31:13]** Yeah. By accident.

**[31:14]** It was through a static website actually. Okay,

**[31:17]** But they actually published this all that in their git repository I think. So you can still do mistake even more if you're using code because you can publish things without not but then I mean the way to get there is harder than if you actually use

**[31:37]** But I think we should agree that a community website as a static website with a complete public repository is yeah even if there's something like a CSS error or there's a dead link or Anyone can now contribute and improve this. Is this something like with Quarkusio? Can I go to the homepage, find a typo and fix it?

**[32:03]** Please do. Please do. [laughter] There's Yeah, we've got a GitHub link at the bottom in the footer. Yeah, anybody can clone it and improve it.

**[32:11]** That's good and bad at the same time. It depends on what you're trying to do. Some might want to have their s source private. they might want to have article ahead of time that are on public that could be required and with GitHub at least with the public

**[32:28]** License you can do that but they have a business license where you it's really it's not that expensive I think it's $50 a year or something and then you can have a private hosted GitHub and a public GitHub pages websites which make a very good solution if you want your source to be private.

**[32:53]** Yeah. For instance, if you have a web shop like website for your company and still have the sources separated from what is actually published. Okay. Based on this whole discussion about how easy it is to make a static website, is there still a business case for WordPress and yeah, there are many other commercial content management systems. Where do you still see a fit for those? As far as I know, I think WordPress has been used in the wrong sorry in the wrong way for years. they've been using it as a backend as services as for managing users. It's not been used for as a content management system. That's why also they always using plugins for everything. and that's bad because that's not the way it's supposed to. It's just

**[33:48]** It's so easy. And if we can continue on in that way of making static easy then for sure I think it will be the best way forward.

**[34:01]** Yeah, I think where we are right now the workflows because they involve git so much and git can be challenging if you're not used to git and a lot of the UI tools for git somehow make it worse. I don't I don't know how quite how that happens. so I think I you know I do see a place for these hosted services and for ones that really do sort of hide away the back end, but I think for a technical audience like us, it's it's so much more compelling to do it static, to do it with the flat files, to do it with GitHub. It's it's got just so many advantages. Mhm. And for people who are comparing, so having an easy way to edit content without needing to hit, that's one thing. So

**[34:56]** I'm looking to my marketing colleagues, for instance, who are not really familiar with this kind of stuff. Something else missing.

**[35:05]** So for instance, we should become we should just start hosted service that just abstract all those layers, but still use it. And I think that will be the solution for everyone.

**[35:15]** Mhm.

**[35:15]** Maybe that will be my next [laughter] next.

**[35:18]** Yeah.

**[35:19]** But then indeed only to add content, not to manage the whole website and to again introduce security issues with managing users and products and stuff.

**[35:29]** We will just use GitHub behind the scene and the exact same flow just you hide it be behind hosted service. And I think if you also add comments and all those stuff as hosted services that just plug in your static website then you have the toolbox that WordPress provides but with sustainable tools.

**[35:51]** Mhm. One thing I was missing is the read counter. So every time a page is loaded that is but that was easily added with cloth flare and the database and the worker. So we just add a counter. We have no privacy concerns because we don't track IPs, who you are, what the page was you visited before. It's just this page, this URL was watched. It's just at the counter plus one. And again, each time the website is built and we now do this four times a day to update this time. We get all the numbers back from Cloudflare and Atom as as static content. that kind of stuff is missing in a static website, but they're so easy to solve and again for a very very low price cloudflare you can get for free.

**[36:38]** Yeah, most of the time you can use free tier because those services are not using a lot of resources. if you look at Google cloud platform free tier it provides a lot and you can use Quarkus to do those services and make them scale to zero

**[36:54]** Because

**[36:56]** They're not visited every day most of them they just like u can be spinned up at any time you need them and then you you're not paying for them you're just paying when they use which is quite logical in the end

**[37:09]** Okay so it's very challenging and fun times for anyone who wants to build a website. Plenty of choices even in the AVA system where we all live and where we love to use our tools. Okay. Anything you want to add?

**[37:23]** You should switch to rock. [laughter]

**[37:26]** I I'll create an issue for you and assign it to you. [laughter] no, that's that's the next thing. We can do a new podcast then. Okay, [laughter] that's it for this episode of the Foojay podcast. Andy and Holly, thank you both for walking us through the trade-offs between CMS systems, content management systems, and static websites, and for being honest about where these approaches are still fall short. not a lot of course. Thank you also to my listeners for tuning in. If you enjoyed this conversation, please subscribe to the Foojay podcast on YouTube or in your favorite podcast app so you don't miss the next episode. See you next time. Give me a foo. Give me a J. Give me the friends of OpenJDK.
