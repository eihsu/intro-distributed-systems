## Proposal for Workshop Experience:
# Introduction to Distributed Systems for Scaling Real-World Applications

## Basic Idea

Participants learn about large-scale distributed systems, and build up
their own system in a simulated asynchronous environment, progressing
from a single server with a single database to a load-balanced network
of workers interfacing with distributed (sharded) database.  They will
configure the network and create their own hashing functions in
Python, building up the maximum number of requests per second they can
handle.  No prior experience of any kind is required.

## Motivating Concepts

Adult professionals may have some familiarity with "the cloud" or
"data centers" as users, but want to learn more about how they work;
even in the tech community there is a lot of obfuscation around
back-end engineering and operational deployment, as the front end is
the most common point of entry for new hires, and system architecture
remains in the hands of a few senior engineers.

For youth there is a real opportunity to supplement pure "coding" (the
skill) with some exposure to "computation" (the science), and share
some of the fun and interest of working with algorithms, and the sense
of accomplishment in diagnosing and improving a system to the point of
high performance.  Undoubtedly any exposure to any kind of tech is
beneficial, and there are many good reasons why the front end has been
emphasized for beginners (opportunities for artistic creativity,
easily visible accomplishments).  Here the goal is to give some
exposure to the front end while retaining these same user-friendly qualities.

## Pitch to Participants

If you have ever saved a document to the cloud, or posted to a social
media profile, then you have used a distributed system
built from thousands of individual computers working together to
process your transaction.  Figuring out the right design to organize a
system that can handle thousands of users per second, while surviving
outages and other catastrophes, takes plenty of creativity,
investigation, and experimentation.

Whether you are a absolute beginner looking to get your feet wet with
some programming, or an experienced professional who would like to
learn more about back-end engineering and the joy of tinkering with
algorithms to build better and better performance, this is the
experience for you.  Participants will use simple Python code to test
their distributed system designs under different patterns of
simulated traffic, and the experience is designed so that
anyone--including complete non-programmers can do it!

The Ladies Learning Code Introduction to Distribute Systems is a
hand-on experience.  During the workshop, you will learn:

+ General design principles for large-scale systems that are
  transferrable to other areas including robotics, AI, streaming
  media, and data science.
+ Learn how to configure, benchmark, and improve a distributed system
  using a software simulator using the same principles that power the
  data centers behind popular web sites, apps, and the cloud.
+ What resources are available if you’d like to continue learning at
  home--and we think you will!

## Opportunities

+ Exposure to an important area of modern computation that is seldom
taught to beginners (or even to university students or experienced
engineers.)

+ Satisfaction to build up own system within an open-ended environment
and progressively improve performance by applying concepts as they are
presented in slides.

+ Learn some programming in a general-purpose language (pure Python)
to get results without having to go deep into the language as a whole.

+ Experiment with algorithms and some of the science and creativity
behind computation, and not just pure coding.

+ Finally learn what is going on in "the cloud" or the datacenter.

## Risks and Mitigation

+ Potential attendees may be less familiar with backend engineering
(compared to web site design or games) and lack initial interest.
  + Appeal can tie subject to more familiar concepts like cloud or
  ATMs or streaming video sites (implemented as concurrent distributed
  systems built on data centers) and trendy buzzwords like AI or
  Hadoop (whose successes are built on the rise of data centers with
  large distributed data sets.)
  + Pitch and title should emphasize that experience is for
  beginnners.


+ Existing distributed environment and concurrency simulations my be
too complex for learners to navigate and for instructors/mentors to
support.
  + Course developer, upon evaluating existing libraries/platforms,
  should probably develop standalone custom simulator limited to
  course requirements, that can be deployed (ideally) by including a
  single pure python file.

+ Recruitment of instructors and mentors may be inhibited by
underrepresentation of backend engineers within pool.
  + Call for support can emphasize introductory nature of course, and
  posting example project to github can demonstrate minimal required
  expertise.

+ Concurrency is hard!
  + Use a highly simplified deterministic simulator.
 
# Outline of Content

FIXTHIS

[v1](#version-1-single-server)

[v2](#version-2-replication)

[v3](#version-3-configuring-replication)

[v4](#version-4-partitioned-database)

[v5](#version-5-consistent-sharding)

[v6](#version-6-effective-hashing)

[v7](#version-7-fault-tolerance)

[v8](#version-8-go-for-it-)

# Simulated Example Projects

---
## Version 1: Single Server
### (Instructor-Led) *Demonstrate baseline system, introduce the simulator.*

Minimal system consists of a single database and a single server
(worker) that connects to the database in order to process a stream of
incoming requests.  Show how to set up the system with four lines of
code, and run it through the simulator with different loads; interpret
log messages and measure throughput.

#### Example code:
```python
import simulator

db = Database()
w = Worker(db)
sim = Simulation(w)
sim.run()
```

In the example code, machines are created using class constructors of
same name, and the database is connected to the worker by passing it
as an argument.  Finally, we run traffic through the single server by
passing it to a simulation as an argument, and then call `run()` on
the simulation.

#### Example log:
```
W1 received “alexandra”
DB1 updated “alexandra"
W1 finished  “alexandra"
W1 received “peter”
DB1 updated “peter”
W1 finished “peter”
W1 received “siddiqui”
W1 received “mary”
W1 received “tao”
DB1 updated “sidiqqui”
W1 finished “sidiqqui”
W1 received “lin”
W1 received “carmen”
DB1 updated “mary”
W1 finished “mary"
...
Simulation complete, runtime 50 seconds.
```

In the example log, the worker initially processes requests as quickly
as they come in (as for Alexandra and Peter.)  With time and/or
increasing simulated traffic rate, the worker's queue starts building
up with multiple requests staged for processing.  Further points to
demonstrate:

+ Show failure (queue overflow) by increasing the rate of traffic.
+ Show total time for system to process all the requests at different
  rates, estimating cost of queing/dequeueing by comparing early
  requests with later ones.  (This is the same multitasking cost we
  incur to varying degrees as humans.)
+ Figure out maximum traffic rate that system can handle before
  failure.
+ Show that updates are taking effect by calling, for example,
  `sim.lookup("peter",db)` to retrieve a simulated social media
  profile.  Data should provide for complete profiles, so that later
  we can observe the necessity of handling consistency in the sharded
  db: calling a lookup on a shard will yield an incomplete (or
  incorrect) profile.

[\[ back to outline \]](#outline-of-content)

---
## Version 2: Replication
### (Instructor-Led) *Distribute work over multiple machines by horizontal fan-out, introduce load balancing.*

System expands to five identical workers behind a load balancer that
dispatches requests to them at random.  The five workers are all
connected to a single database.

#### Example code:
```python
import simulator

db = Database()

w1 = Worker(db)
w2 = Worker(db)
w3 = Worker(db)
w4 = Worker(db)
w5 = Worker(db)

lb = LoadBalancer([w1,w2,w3,w4,w5])

sim = Simulation(lb)
sim.run()
```

#### Example log:
```
Simulation complete, runtime XX seconds.
```

[\[ back to outline \]](#outline-of-content)

---
## Version 3: Configuring Replication
### (Participant-Led) *Benchmark performance, diagnose bottlenecks.  (It's the database).*

#### Example code:
```python
import simulator

db = Database()

w1 = Worker(db)
w2 = Worker(db)
w3 = Worker(db)
w4 = Worker(db)
w5 = Worker(db)
w6 = Worker(db)
w7 = Worker(db)
w8 = Worker(db)
w9 = Worker(db)
w10 = Worker(db)

lb = LoadBalancer([w1,w2,w3,w4,w5,w6,w7,w8,w9,w10])

# Can show shortcuts like loops or comprehensions, i.e.:
# lb = LoadBalancer([ Worker(db) for i in range(10) ])

sim = Simulation(lb)
sim.run()
```

#### Example log:
```
Simulation complete, runtime XX seconds.
```

[\[ back to outline \]](#outline-of-content)

---
## Version 4: Partitioned Database
### (Instructor-Led) *Distribute data by sharding, demonstrate concurrency/consistency challenge.*

#### Example code:
```python
import simulator

db1 = Database()
db2 = Database()
db3 = Database()
db4 = Database()
db5 = Database()

w1 = Worker(db1)
w2 = Worker(db2)
w3 = Worker(db3)
w4 = Worker(db4)
w5 = Worker(db5)

lb = LoadBalancer([w1,w2,w3,w4,w5])

sim = Simulation(lb)
sim.run()
```

#### Example log:
```
Simulation complete, runtime XX seconds.
```

[\[ back to outline \]](#outline-of-content)

---
## Version 5: Consistent Sharding
### (Instructor-Led) *Apply conditional hash function for distributing data.*
#### Example code:
```python
import simulator

db1 = Database()
db2 = Database()
db3 = Database()
db4 = Database()
db5 = Database()

w1 = Worker(db1)
w2 = Worker(db2)
w3 = Worker(db3)
w4 = Worker(db4)
w5 = Worker(db5)

lb = LoadBalancer([w1,w2,w3,w4,w5])

def trivial_hash(name):
  choice = 1
  return choice

def simple_hash(name):
  if len(name) == 1:
    choice = 1
  if len(name) == 2:
    choice = 2
  if len(name) == 3:
    choice = 3
  if len(name) == 4:
    choice = 4
  if len(name) > 4:
    choice = 5

  return choice

# Initially can show:
# lb.set_hash(trivial_hash)

lb.set_hash(simple_hash)

sim = Simulation(lb)
sim.run()
```

#### Example log:
```
Simulation complete, runtime XX seconds.
```

[\[ back to outline \]](#outline-of-content)

---
## Version 6: Effective Hashing
### (Participant-Led) *Create a balanced hash, identifying patterns in data and traffic.*

#### Example code:
```python
import simulator

# [configuration code omitted, same as previous example]

def alpha_hash(name):
  # (Participants can examine traffic and find that disproportionate
  #  number of requests involve a single account.  Can explain caching
  #  as alternative to dedicated machine.)
  if name == "kim":
    return 1

  c = name[0]
  if c < 'h':
    # names starting with [a-f]
    return 2
  if c < 'g':
    # names starting with [g-n]
    return 3
  if c < 'g':
    # names starting with [o-s]
    return 4

  # all others, i.e. names starting with [t-z]
  return 5

lb.set_hash(alpha_hash)

sim = Simulation(lb)
sim.run()
```

#### Example log:
```
Simulation complete, runtime XX seconds.
```

[\[ back to outline \]](#outline-of-content)

---
## Version 7: Fault Tolerance
### (Instructor-Led) *Introduce failures, then demonstrate failover by vertical replication.*

#### Example code:
```python
import simulator

db1 = Database()
db2 = Database()
db3 = Database()
db4 = Database()
db5 = Database()

w1 = Worker(db1)
w2 = Worker(db2)
w3 = Worker(db3)
w4 = Worker(db4)
w5 = Worker(db5)

w6 = Worker(db1)
w7 = Worker(db2)
w8 = Worker(db3)
w9 = Worker(db4)
w10 = Worker(db5)

lb = LoadBalancer([[w1,w6],
                   [w2,w7],
                   [w3,w8],
                   [w4,w9],
                   [w5,w10]])

sim = Simulation(lb)
sim.set_failures(True)
sim.run()
```

#### Example log:
```
Simulation complete, runtime XX seconds.
```

[\[ back to outline \]](#outline-of-content)

---
## Version 8: Go for It!
### (Participant-Led) *Experiment and make open-ended improvements, pursue high scores.*

#### Example code:
```python
# [open-ended combination of previous concepts and
#  not-yet-introduced simulator features]
```

#### Example log:
```
Simulation complete, runtime XX seconds.
```


