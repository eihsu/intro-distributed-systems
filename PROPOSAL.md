### Proposal for Workshop Experience:
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
  + Pitch should tie subject to more familiar concepts like clouds or
  ATMs or streaming video sites (implemented as concurrent distributed
  systems built on data centers) and trendy buzzwords like AI or
  Hadoop (whose successes are built on the rise of data centers with
  large distributed data sets.)
  + Pitch and title should emphasize that experience is for beginnners.

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
 
# Course Outline and Simulated Project Example

FIXTHIS


## Version 1 (Provided to Participants): Single Server

#### Demonstrate a baseline system, introduce the simulator.

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
passing it to a simulation as an argument, and calling `run()`.

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

## Version 2 (Developed Together): Replication, by Horizontal Fan-Out

#### Distribute work over multiple machines, introduce load balancer.

System expands to five identical workers behind a load balancer that
dispatches requests to them at random.  The five workers are all
connected to a single database.

#### Example code:
```python
import simulator

db = Database()
w = Worker(db)
sim = Simulation(w)
sim.run()
```

#### Example log:
```
Simulation complete, runtime 50 seconds.
```


## Version 3 (Participants Develop): Configuring Replication

#### Benchmark performance, diagnose bottlenecks (database).

#### Example code:
```python
import simulator

db = Database()
w = Worker(db)
sim = Simulation(w)
sim.run()
```

#### Example log:
```
Simulation complete, runtime 50 seconds.
```


## Version 4 (Developed Together): Partitioned Database (Sharding)

#### Distribute data, naturally identify concurrency/consistency challenge.

#### Example code:
```python
import simulator

db = Database()
w = Worker(db)
sim = Simulation(w)
sim.run()
```

#### Example log:
```
Simulation complete, runtime 50 seconds.
```


## Version 5 (Developed Together): Consistent Sharding

#### Apply conditional hash function for organizing data.

#### Example code:
```python
import simulator

db = Database()
w = Worker(db)
sim = Simulation(w)
sim.run()
```

#### Example log:
```
Simulation complete, runtime 50 seconds.
```


## Version 6 (Participants Develop): Effective Hashing

#### Create balanced hash, identify patterns in data and traffic.

#### Example code:
```python
import simulator

db = Database()
w = Worker(db)
sim = Simulation(w)
sim.run()
```

#### Example log:
```
Simulation complete, runtime 50 seconds.
```


## Version 7 (Develop Together): Vertical Fan-Out for Fault Tolerance

#### Introduce failure and (combine code from then and now)

FIXTHIS

#### Example code:
```python
import simulator

db = Database()
w = Worker(db)
sim = Simulation(w)
sim.run()
```

#### Example log:
```
Simulation complete, runtime 50 seconds.
```


## Version 8 (Participants Develop): Go for it!

#### Open ended design and experimentation, pursue high scores.

#### Example code:
```python
import simulator

db = Database()
w = Worker(db)
sim = Simulation(w)
sim.run()
```

#### Example log:
```
Simulation complete, runtime 50 seconds.
```


