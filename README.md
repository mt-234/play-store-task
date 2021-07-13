# PLAYSPORTS Backend Developer Test
​
Thank you for taking our developer test. The goal is for us to get an accurate estimation of both your technical expertise as well es your working style.
Thus, this test is split up into two parts: first, you will work on the tasks outlined below. Then, we will schedule a call where you will have the opportunity to walk us through your work and explain your thoughts behind it.
​
Please create a branch out of this `main` branch, apply your changes (i.e., fill in your solutions in this `README.md` and/or create separate files as you wish).
Once done, please create a pull request into the `main` branch and request a review from us on GitHub.
​
As an orientation, you should aim at spending about 2-3 hours in total for both tasks together (if you are done in much less time or want to take more time, that's fine too, though!).
​
**Important**: if you have any questions, do not hesitate to reach out to your interviewer. We might have missed something in the task descriptions here, so it might well be the case that you need some additional info to proceed. If you can't think of any solution in reasonable time, please do not Google for a solution (especially in task 2.3). Instead, contact us, so we can give you a hint or another task. (You may use Google or any other resource to do research for your solution of course, though!).
​
## 1. Programming Tasks
​
### 1.1 WarmUp
You are given an array of strings with some small and large letters in JavaScript.
```javascript
const array = ['a', 'b', 'c', 'D'];
```
Please produce a string by removing upper case characters, appending '1' to each string and then merging the strings with the comma separator.
So in the example, the result would be `'a1,b1,c1'`. **Do this as succinctly as possible**.
​
You can type the solution here:
```javascript
const inputArr = ['a', 'b', 'c', 'D']
let finalArr = inputArr.filter(character =>  character == character.toLowerCase()).map(character=> character = character +'1')
console.log('finalArr: ', finalArr);
```
​
### 1.2 Algorithm: Connections
You are given a list of connection events of a virtual PLAYSPORTS event session (i.e., a live stream).
A connection event has the following structure:
```typescript
type ConnectionEvent = {
    connectionId: string;       // a unique id for this connection (the 'connected' and 'disconnected' events of a connection will have the same)
    userId: string;             // the id of the user who initiated the event
    timestamp: Date;            // the date when this event happened
    type: 'connected' | 'disconnected';     // whether the user connected or disconnected
};
```
​
The goal is to calculate a set of stats for the virtual event session, i.e., how many unique users connected, how many connections were initiated, etc.
Your function should return an object of the following type:
```typescript
type SessionStats = {
    /**
     * The number of connections that were initiated (started)
     */
    numConnectionStarts: number;
​
    /**
     * The number of unique users that were connected to the session at any point (at least once).
     */
    numUniqueUsers: number;
​
    /**
     * The maximum number of users that were simultaneously connected, i.e., that had overlapping connections.
     * Example: If user A connects at 12:00 and disconnects at 12:10, and user B connects at 12:05 and disconnects
     *          at 12:15, this would be a maximum of 2 simultaneously connected users
     */
    numMaxUsers: number;
​
    // the below values are optional (only calculate them if you have enough time and enjoy it ;))
    /**
     * The percentage (share) of all connections that were still active after being active for 30 seconds
     */ 
    connectionsActiveAfter30s?: number;
};
```
​
Implement a function that takes the list of connection events in any order and returns the stats that has optimal time complexity.
Name and explain the time and space complexity.
​
**Constraints**
* You may use additional space.
* For date comparison and manipulation or getting some fields like the seconds of a date, you can use any NPM library or just use pseudo-code functions (e.g., you can use something like `getSeconds(Date)`). You can compare dates using `compareAsc(a, b)`, which returns 1 if `a` is after `b`, 0 if they are equal and -1 if `a` is before `b`.
* You can assume that the events are either empty or they contain pairs of 'connected' and 'disconnected' events that share the same `connectionId`.
​
You can use the template below:
​
```typescript
// add npm package to calculate date things named: moment
  calculateStats(events: ConnectionEvent[]) {
    // we assume that events sort by timestamp -1
    try {
      let finalStats = {
        numConnectionStarts: 0,
        numUniqueUsers: 0,
        numMaxUsers: 0
      }
      if (events && events.length > 0) {
        // The number of connections that were initiated (started)
        let getConnectedUser = events.filter(e => event.etype && event.etype === 'connected')
        finalStats.numConnectionStarts = getConnectedUser.length;

        // The number of unique users that were connected
        const uniqueByUser = [...new Set(getConnectedUser.map(event => event.userId))];
        finalStats.numUniqueUsers = uniqueByUser.length;

        // add 10 min to current timestamp (i.e., 'A' user connected 2:30 then we check is there any user that connected between 2:30 to 2:40 ). 
        let connectedSimultaneously = [];
        const minutes = 10;
        events.forEach(event => {
          if (event.etype) {
            let startDate = moment(event.timestamp).toISOString()
            let endDate = moment(event.timestamp).add(minutes, 'minutes').toISOString()
            let findUser = events.filter(ev => {
              return moment(moment(ev.timestamp).toISOString()).isSame(startDate) || moment(ev.timestamp).isBetween(startDate, endDate);
            })
            if (findUser && findUser.length > 1) {
              connectedSimultaneously = [...new Set([...connectedSimultaneously, ...findUser])]
            }
          }
        })
        finalStats.numMaxUsers = connectedSimultaneously.length;

        return finalStats
      } else {
        return finalStats;
      }
    } catch (e) {
      throw new Error('not implemented');
    }
  }
```
​
## 2. Systems Design Tasks
​
### 2.1 Systems Design: Slow Endpoint
Assume there is an endpoint in your API that takes around 2 seconds of work to complete (for example, a couple of complex database queries).
This endpoint is called by a mobile app in response to a user clicking a button. 2 seconds is quite a long time and the user will feel like the app is very slow.
There are two scenarios:
1. The response of the endpoint sent to the device depends on the work being done.
2. The response of the endpoint sent to the device *does not* depend on the work being done.
​
Please list 2 approaches on how to make the endpoint faster for the first scenario and 1 approach for the second scenario (i.e., 3 approaches in total).
**If any conditions need to be met in order for your approaches to be viable, please name them**.
​
You can use the space below.  
​
**Approaches in Scenario 1**:
1. If there is complex database queries that takes some time to give response then we can use "Indexes" mechanism to perform query faster. Or If we already use this mechanism then DB Indexes do not have in proper way.
2. Sometime API enpoint have some special task like sending as email then we can use other third party service like Redis. So we just focus on the main task and we give response to request (Without waiting to the other task that definitely will done when made request).  

​
**Approach in Scenario 2**:
1. Sometime user internet speed also affect slow for application performance. Also database server CPU and Size also affect the slow query performance.
​
### 2.2 Databases: 2 Queries
Imagine you have two SQL queries A and B. A must happen before B. If any of the two queries fail, the whole query should fail and none of the changes of either A or B should be applied to the database. Write one or multiple SQL statements that achieve this task. A and B have already been added below:
```sql
we can achieve this using rollback/manual-rollback functionality. If any query fails all other previously query will roll back
BEGIN TRANSACTION
 
  Query A;
 
  Query B;

 
COMMIT TRANSACTION
```

​
### 2.3 AWS Architecture Design
In this task, assume the PLAYSPORTS API is currently deployed from some single self-hosted server that sits in our CEO's basement (hint: this is actually not the case).
Now, with increasing amounts of traffic, you are assigned to design a scalable and secure server architecture on AWS that we can migrate our API to.
The API itself is a monolithic Node.js/Express/TypeScript app that uses a MySQL database and a Redis cache, both of which currently run on the same machine (hint: this is also not actually the case).
​
Keep in mind the following system requirements:
* Incoming requests go to https://api.play-sports.org (note the SSL/TLS encryption).
* The architecture should scale automatically with demand (if many requests are coming in, increase capacity, and if it traffic decreases again, decrease capacity)
* The database should have read replicas.
* All servers should be protected from unwanted access (i.e., you should ahere to the [principle of least privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege)).
* Environment variables should be easy to change.
* Deployment of a new version should be automated as much as possible.
* Server maintenance and setup should be automated as much as possible.
* Deployments should cause zero downtime.
​
**Constraints**
* You may use any AWS service like EC2, RDS, ECS, Lambda, ...
* If you are making any assumptions, please note them explicitly.
* Please describe your architecture using full sentences and not just keywords. You can provide a diagram as well.

***We can manage AWS Architecture Design things as per below***

***Stack***
* We can use NodeJS + ExpressJS + TypeScript and ESLint + Prettier for coding standard and formatting.
* Database can be either MySQL or MongoDB. 

***Data storing***
* Approach 1: We can use replica to store important data, once those information has been stored into our replica then only we will store into our main database otherwise that transaction will be rolled back.
* Approach 2: We can add Read replica for our AWS configuration which will be automatically synchronizing the replicas from the main instance.
* For getting more speedy response we can set up Redis server, which will provide response directly whenever same request being made so in that case we can reduce extra overload on API server.

***API hosting***
* NodeJS can be hosted on AWS EC2

***Load Balancing / Scaling***
* Our server configurations may be proper for ideal cases but on some days if traffic increases then for short duration we should not increase capacity permanently. In such cases we can opt for either load balancer or dynamic scaling service by AWS, load balancer would add another EC2 instance based on the traffic load calculation and decrease once we get back to normal routine traffic whereas dynamic scaling will try to increase and decrease the capacity of the existing cpu itself.

***Deployment***
* We can add CI/CD configuration YAML file to enable continuous integration and deployment of our code, based on the rules added in YAML file it will run the scripts once we made commit into our repository. We can define what needs to be done on success or failure of scripts to avoid server crashes.
