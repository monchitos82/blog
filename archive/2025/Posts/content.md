# Postmortems: (05/25)

One of the best parts of Site Reliability **definitely** is: Executing blameless postmortems. It's one of the hardest too. Taking care of figuring out what went wrong and trying to come to action to fix what is broken is no small challenge and we all will be sloppy trying it the first time. There is a great shortcut to get there though, and it is actually documented: Reading engineering blogs, and playing the [wheel of misfortune](https://sre.google/sre-book/accelerating-sre-on-call).

A postmortem goes beyond trying to find the ugly truth and following a template, although having a good template and a good postmortem document is a non-trivial task. A memorable postmortem is that one where you leave the room with conviction of: a) now you know way more than what you did when you wrote the document draft, b) your respect for your collaborators and their work has a new high and c) you're convinced you are going to make things better from that point. Truth is, this is not always achieved and especially point _c_ is a pain in the ass, because pressure on releasing new things is always pushing flaws downward in everyone's queue.

So why are postmortems hard anyway? Where is the process turning into something difficult?

Postmortems are a blameless process (or they should be). Keeping it that way is a titanic effort. Nobody likes to hear their work is dogshit and their sloppiness causes an outage, regardless, those things happen and the process of figuring out what happened in detail, when done right, brings out all the dirt from under the rug and sometimes, it gets ugly.

Postmortems require a fair amount of effort, both because they require focus, and they require to do an investigation fast. It serves no good to schedule a postmortem meeting a month after an incident happened, by then the world has moved on. So, whoever oversees gathering people needs to do both: a thorough investigation and organize a meeting with a similar sense of urgency to what it took to solve an incident that triggered a postmortem.

Postmortems derail a ton of work. Incidents happen but making sure they don't come back requires revisiting a lot of work and disrupting the release process of features, it demands commitment from engineering and product parties to prioritize site up above everything else. As with any important decision, it is not coming for free.

I will steer a little bit here and talk about what is a postmortem anyway, how it is expected to go and what made a postmortem memorable in my experience. My experience is limited though, so this should be brief.

When systems and infrastructure fail, failure brings a learning opportunity. Organizations that follow an incident management practice will identify the steps to follow from the moment an incident is detected until it is resolved, however, the root cause of an incident is not always crystal clear and while in the old ITIL world closure would come after investigation, a thorough root cause analysis and long term remediation may remain unclear; probably that's why ITIL themselves formally introduced the process.

The blameless postmortem process as a practice was popularized by Google when they introduced SRE, and it is a powerful tool to understand what has gone wrong rather than figuring out "who" dropped the ball. As I mentioned before it is easier said than done, egos and power dynamics (a.k.a. red tape / office politics) often play against. I think that one of the best practices I witnessed when trying to make this blameless and a learning endeavor is **keeping your organization values present**. I think the reason why is because you already have a common goal which is to honor those values and of course you have that problem that's stealing away your sleep, and hell, you want it gone!

But how do things flow exactly?

```
your system shits the bed
  |
  |__ you get alerted and you start an incident resolution process
        |
        |__ you find the source of the problem, you fix the problem
              |
              |__ you want to make sure this is not reoccurring, you start investigating what happened indeed
                    |
                    |__ you document your findings and establish a timeline
                          |
                          |__ you bring together all stakeholders of the system | business
                                |
                                |__ you unearth the details of the incident and make sure you answer:
                                       - what went wrong
                                       - what went right
                                       - where luck was on your side
                                       - where it wasn't
                                       - what could have been done to prevent it
                                      |
                                      |__ you identify all the broken parts of your system and process, agree to a fix, set a timeline and a priority
                                            |
                                            |__ you follow up the fixing process
                                                  |
                                                  |__ you recover your sleep... or maybe not
```

Postmortem as a process, peaks when a meeting occurs. Memorable meetings are not those where people point fingers, but those where people recognize things are broken and agree to fix them properly. Basically, a memorable postmortem is one where you agree to **make things right**. Sadly, not all leaders and top engineers want to go this way, tech debt is often what comes in exchange for moving fast, other people with dubious morals will do what they can to go unscathed or simply suggest sweeping things under the rug. But even in the ugliness of these situations, great engineers arise, people calling out this as a deviation from your core values or simply owning the problem. When this happens, you realize not everything is rotten. Is this it? Just the old good vs evil? No advice here? Well, I can't spill the secret sauce, but basically try to run a meeting like this:

- Come prepared. Your document is clear, the details are there
- Have people read your doc
- Walk through your timeline, root cause analysis and start asking for gaps
- Have people question what they read, fill in the blanks, improve the picture
- Have the experts chime in, this is where their wisdom helps
- Ask the questions from the diagram:
    - what went wrong
    - what went right
    - where luck was on your side
    - where it wasn't
    - what could have been done to prevent it
- Set action items
- Find owners, if nobody wants to **own the problem**, ask more questions, call for accountability
- Set a timeline, document again
- Follow up your document, empty promises bring problems back and you as the person in charge of preventing this from happening again, should oblige
- If the problem rises again, your investigation went wrong, it will happen, sometimes it is just symptomatic, sometimes it really is a bad fix, learn from it!

Make sure that no matter what, there is nobody pointing fingers or just ranting, you want a problem fixed, this is your duty.

But wait, who runs the meeting after all?

In my brief experience, incident management leads (the person who coordinated the fire fighting) is the right person, however, there's nothing wrong with passing on the lead to a more experience engineer or a manager during the meeting, after all this is not an exercise to take credit, you want things fixed above everything else, your site stability, your system, your alerting, your rest and your sanity are at stake here.

What happens after this?

Well, if you write a good postmortem not only you helped solve a problem, you have good material for your engineering blog, a good tale for your nerd friends or a justification for your eye twitching.


# Containers (06/25)

I have to start here by saying I am no expert in containers. I have been around to watch and participate in the transition from _VMs_ to containers. I have used many and yet not enough of them. This entry is another one of my tiny ass cheat sheet entries by the way.

Anyway, my interest in virtualization and hypervisors dates a couple decades back. I started when vmware was the shit and back then I had no clue what to use it for. I recall having a client running in my budget machine and never delving into what I could do with it because my understanding of _Linux_ was more than limited. Then I graduated college, started delving into _Solaris_ and started hating to do all my work via _putty_ so I ended up finding a way to use virtualbox and a USB to get a terminal and an _IDE_ that actually felt comfortable rather than using _notepad++_ and _putty_ to get things done. So back then all I understood about isolation were permissions and root privileges, never thought (or learned) about chroot and what a good tool it was to break apart an application path, so for sure _cgroups_ were out of the map.

Then _docker_ came along and it was pure magic, the idea of having an image where you could install, test and ship your code was very exciting, specially when most of my work was done by a playbook, so having a way to automate the build and a way to ship in a tested manner where no installation was required felt like a big time saver. But then one wonders, what is this container actually doing and why do I still use virtualbox anyway? That was a question it took me some time to answer. Articles and demos all mentioned something about borrowing what is part of the kernel, but never mentioned which parts. Well, that's where that explanation sucks because to be fair nothing is borrowed, in the simplest term, a container is just creating a partition within the _OS_ resources to create isolation. Had this been told just like that the concept would be super simple to digest, but you know, people like to sound smart.

I think I never pulled apart from the abstraction _docker_ created and what actually was going on underneath until I stumbled upon a place where containers were self managed! What? You don't use _docker_ for that? It took no time to see _docker_ falling out of fashion and kubernetes ramping up, still, the notion of what is being **used** from the _OS_ itself was intriguing without understanding what is `libvirt`, what's _LXC_, what are _namespaces_ and _cgroups_. So let's try to dig into those things, shall we?

I start with `libvirt` because it is a quite useful _API_ to communicate the _OS_ with different hypervisors, not to mention giving us tools for management of resources. This comes handy when we try to understand the concept of an orchestrator, although this might feel low level, the tooling and the API we have to work with hypervisors, including _LXC_ is key for managing what the container platform creates. I don't think the mentioned library has much to do (if anything) with containers, still it is worth mentioning, because part of the need for containers comes from the need to communicate to parts of the kernel through an API like this one.

Moving on to _LXC_ and why using it paired with `libvirt` can be useful to get a similar functionality to _Docker_, we need to understand what the _Linux_ containers project actually is. _LXC_ is not really what introduced the isolation it is going to work with, but it has a nice way to abstract it so we can focus on defining resources and types without delving into what components of the kernel itself is leveraging to provide isolation for an application. No wonder _docker_ started building around it when it started. But trying to dig deeper, can we do our own container by hand?

Well, circling back to the "self managed" mention, yes: Isolation rather than virtualization (we should always make that distinction when we talk about containers) means that we provide the mechanisms from the kernel itself for an application to run without any awareness of what runs outside its restricted space. How is that achieved? through a set of features from the kernel, of course: capabilities, _cgroups_, _namespaces_, policies and _filesystem_ isolation (chroot comes into play again).

But why or how does this work?

Let's think about an application that requires a certain set of libraries, directories, processes and calls to run. If we already have a kernel that provides its scheduler and sys-calls, we don't really need to worry about that layer. If we however try to use _PID_ 1 for this application and limit what runs on the _OS_ to a given process, we need a way to let that application "_believe_" that it is the only one running. Using concepts like _namespaces_ and _cgroups_ that can be achieved, _namespaces_ will _tag_ a set of resources to be used for a specific process and limit its view, what the process can see, to what _namespaces_ has _tagged_ (in a _OS_ structure point of view, this includes the _PID_ tree, networking, ipc, hostname, user mapping and mounting points just to name some, depending on what you define using `unshare`) while _cgroups_ will reserve and limit the resources that are provisioned to the process (you need `cgroup-tools` to get this working since you need a cgroup created using `cgcreate` and add resources using `cgset`). Is this enough to contain a process? To some extent it is, however the restriction is not limiting access to sys-calls and other critical parts of the _OS_, thus, the use of capabilities and _apparmor_ policies are needed not only to isolate a process, but to restrict it from operating beyond its own scope (we can see how this one works using `showpcaps` on a _PID_ and set it using `capsh`).

So is _docker_ needed at all? What about _kubernetes_?

Well, _docker_, just like _LXC_ abstracts and simplifies the operation with containers, just like libvirt and the _docker_ daemon simplify the orchestration at host level. Why is this the first time I mention _kubernetes_? Because _kubernetes_ is not really part of this orchestration, it lives and operates on a different layer, where groups of containers (or machines) are the abstracted resources so basically executing this same concept at a different scale of resources.

In my experience, all that is needed, all the "magic" is provided by the _Linux_ kernel. We are sold this as a breakthrough but this magic has been around since 2002 and widely adopted in 2008. I have used "in-house crafted" versions of these orchestrators and containerization platforms, they worked and worked quite well. Am I suggesting not using _docker_ or _kubernetes_? Not at all, these platforms are convenient, especially paired with cloud providers, however, they mean complexity just as using AWS means complexity managing resources through a non-standard _API_, yet they are great enablers for scalability when done right, and a strong implementation of what these concepts propose on resource isolation.

Other parts I should mention?

Sure: _UFS_! Although not necessary, the _union-filesystem_ is a layered approach where space is managed with the intent to prevent copying and duplicating files across containers. This provides not only access to different _filesystems_ through a unified view but it allows access to parts of the container like the image where files (read only) can be accessed by different containers, that's why it is a key part of _docker_, however, it is not an exclusive approach of a layered access _Fylesystem_ for user spaces, _LXC_ has its own implementation, however the key purpose of each is different.

And that's it, containers are all about local isolation. Hypervisors are a different animal, they implement their own microcode, drivers and access to resources so while they can run on top of another _OS_, their purpose is to provide an isolated platform for an _OS_ itself. And this just took me years to understand and explain.

What do I use? Well I use what suits my needs, _podman_ for the time being.


# Application bottlenecks (08/25)

I suck at Java. I have never written production grade Java code. But I have gotten to understand the Java virtual machine (JVM), especially because I was mentored by brilliant people who understand the language and the virtual machine inside out. I think one of my best opportunities back at my previous job was around understanding bottlenecks and tuning a JVM. Tuning a JVM is not something we should do out of ignorance though; the JVM defaults are quite effective when dealing with regular problems. I will not write a guide on how to profile projects or picking flags to tune the JVM, especially when it is something I am not doing on a daily basis, however I wanted to share some advice on getting started with profiling and understanding performance from more than a CPU utilization graph perspective.

I should start by mentioning that utilization of resources is not the same as saturation; if you see a CPU is being used over 80% that is not an indicator of trouble, quite the contrary, however, if you throw in the mix latency concerns and garbage collection, well, your problem (if any) turns interesting.

One of the things I learned when working with cgroups and old JVM versions is that understanding the limits of an application is basically the first and only thing we need to do when fitting a process to its assigned resources. There are interesting reads about JVM ergonomics that will suggest we want to let the virtual machine handle the hairy parts, and it is correct for the most part, yet understanding is non-trivial when containers are involved. It's also important to understand the architecture of a program to figure out how and why it solves a problem the way it does.

So let's say you have a simple program that scans an array of objects and uses a library to rank them and build a recommendation. This sounds like something that would be CPU intensive but manageable, except when you run it you notice that latency randomly spikes and you don't see how or why. Looking at logs is not effective since you have no errors coming, just latency, then what?

I dare to say, without hesitation, this was one of my favorite parts of being an SRE. Figuring out what's affecting a service and how to push it to perform better is not a one man kind of task, especially if you didn't write the code or have access to it for a start. There is a great set of tools at your disposal and understanding when to use them is key.

1. Graphs - It sounds contradictory to mention graphs just after I said that graphs will not get us somewhere in the investigation, however, if well instrumented, graphs are quite useful resources, especially when they can be overlayed. Performance problems (_ideally_) are non-persistent, but influenced by outstanding factors. A graph is a good way to understand when and where the problem starts, yet these are not flawless, if instrumentation is poor and there are no good metrics, all we may see are spikes at system levels, that's not useful to understand load as a source but as a consequence.
2. Profilers - Profiling a process is a good way to understand a few things in a reasonable time window: workflows, interruptions, processing load and dependencies to mention a few. When doing this in the JVM there is another thing we get to see: the garbage collection (GC). This is very important and can be paired with an amazing tool called flamegraphs, we get to see duration of a process and the depth of dependencies in a call; this alone may not be very insightful of a problem, but it is very insightful on how resources are utilized and helps us gain context on how a program executes and what a program is calling. In the example above, you may not see a problem in the graph itself, especially when profiling CPU utilization, so how to spot it? I think there are two sensitive ways: a) pay attention to the duration of a method execution, is it short some times and very long another? and b) pay attention to the GC cadence. Is it suddenly too frequent? Too long? Is that graph full of yellow bars (or the color scheme you pick for GC)? That's your clue.
3. Memory dumps - If you get to understand the flow of a program, you may get to see what that program is doing when a problem arises. A memory dump is especially useful when that happens. Having insight on the amount of objects, their type and their size is especially useful when detecting a bottleneck. Going back to the example above, if your program fails only when scanning objects containing 4K image files when it is designed to scan thumbnails, well, it makes sense that your program struggles; but sometimes it is less obvious than that, sometimes, it boils down to using objects like boxed numbers or `arrayList`; objects that improve the developer's ergonomics (lazy evaluation) at the expense of memory footprint that over time will become wasteful and saturate pages at a staggering speed, sometimes all you have to do is go back to using primitives or just correcting the size of a heap because you can't fit objects properly. Getting insight on that could be hinted at by the profiler, but is not coming until you are at this step, then it actually makes sense to think about solutions like tuning the virtual machine or scaling vertically.
4. Profilers (again!) - I will mention profilers again. Just like with overlaying graphs, looking at a program behavior under regular circumstances and saturation can be insightful on how the program behaves on the latter. Sometimes a flame graph will show it to us right away.
5. Source code - I think this is especially useful when we think that our resource allocation is correct and that we see that tuning has no effect, why? because performance problems should ideally be solved at the code level. Improving our code performance is key to improving our resource utilization, not the other way around. Most experienced engineers understand this, except that sometimes the re-engineering cost is too high, so making room is faster disregarding how ugly it may get.

This feels like I am just scratching the surface and want to speed run through the topic. I realize this conversation is not easy because not everyone starts at the same level and depth into the conversation really depends on the problem and the tools. So I would like instead to offer some resources and advice if this is the first time you are dealing with something like this:

1. Use the [SRE book](https://sre.google/books/) to get familiar with concepts like what has been mentioned here, saturation? observability? it is all in there
2. Understanding the [JVM ergonomics](https://docs.oracle.com/en/java/javase/21/gctuning/ergonomics.html) is a good read
3. Understanding the [GC types of collectors](https://docs.oracle.com/en/java/javase/21/gctuning/available-collectors.html) and tuning flags is non-trivial but it is one very important thing to do. Memory management and GC frequency are _knobs_ that you can play with, but ideally you need to understand well first. When in doubt, fall back to the defaults, understand the GC periodicity as a necessary sacrifice of performance in exchange of throughput
4. Understanding profilers like [async-profiler](https://github.com/async-profiler/async-profiler/blob/master/docs/GettingStarted.md) and [flamegraphs](https://www.brendangregg.com/flamegraphs.html). to me was a great investment of time. I used to advice using **Minecraft** to learn how to do profiling, I still find it quite useful; sadly, Mojang has made quite cumbersome understanding the flamegraphs (too much obfuscation in their classes nowadays, thank you Microsoft `:-(`).
5. Using [MAT](https://eclipse.dev/mat/) the first time can be intimidating, yet it is your best **free** option. I was fortunate enough to get [J-Xray](https://jxray.com) taught by its author, however it is not a free tool. Regardless, you may want to read their blog and articles, that amount of wisdom for free is a bargain!
6. Understand how to write performant code. Understand the API of your language and **never** overlook simplicity

I think the list above could rapidly grow the more we delve into specifics, my advice, read as you go and try to be one step ahead in the knowledge. Many people focus on finding tools to rightsize containers and forget the basics like Big-O notation or design principles. This of course is not the end of the conversation, and for the sake of brevity I am shying away from discussing off-heap allocation or JNI invocations, but I hope you get the gist, we need to look at our performance journey through a funnel:

```
\=======================================================================/      ---
 \=====================================================================/        |
  \                          Resource definition                      /         Environment adjustments (e.g. full memory allocation) and vertical scaling
   \                         (Container settings)                    /          |
    \---------------------------------------------------------------/          ---
     \                       System metrics                        /            |
      \                      (Monitoring)                         /             |
       \---------------------------------------------------------/              |
        \                    Application configuration          /               |
         \                   (Settings)                        /                |
          \---------------------------------------------------/                 Performance bottlenecks identification
           \                 Application metrics             /                  |
            \                (Monitoring)                   /                   |
             \---------------------------------------------/                    |
              \              CPU utilization ↻            /                     |
               \             (Profiling)                 /                      |
                \---------------------------------------/                      ---
                 \           Memory footprint          /                        |
                  \          (Dump analysis)          /                         |
                   \---------------------------------/                          Design and implementation bottlenecks identification and optimizations
                    \        Code analysis          /                           |
                     \                             /                            |
                      \---------------------------/                            ---
                       \     Running environment /                              |
                        \    tuning             /                               JVM and kernel fine tuning
                         \=====================/                                |
                          \===================/                                --- 
```

Final, and not less important advice I can share: Measure. Load testing and metric analytics are cornerstones of a good service health, not paying attention to this, will not spare you the above, but **it will save you from executing it with high urgency**.
