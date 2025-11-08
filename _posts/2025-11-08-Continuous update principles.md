These are all strategies for updating software applications with minimal or zero downtime. They differ in their approach to risk, cost, and speed.

## 1. Rolling Update

* **How it works:** The new version (v2) gradually replaces the old version (v1) one instance at a time, or in small batches. During the update, the load balancer directs traffic to both v1 and v2 instances until all instances are running v2.
* **Pros:**
    * **Low Cost:** Does not require duplicate infrastructure.
    * **Simple:** A straightforward, often automated process.
    * **Zero Downtime:** The application remains available.
* **Cons:**
    * **Slow Rollback:** If a problem is found, rolling back is also a slow process (must "roll" backward).
    * **Compatibility Issues:** For a short time, both v1 and v2 run simultaneously, which can cause problems if they can't coexist (e.g., database schema changes).



---

## 2. Blue-Green Deployment

* **How it works:** You have two separate, identical production environments: "Blue" (running v1) and "Green" (running v2).
    1.  All user traffic goes to the **Blue** environment.
    2.  Deploy and fully test the new v2 in the **Green** environment (not visible to users).
    3.  When confident, switch the load balancer to direct **100%** of traffic from Blue to Green. This cutover is instant.
    4.  The Blue environment is kept on standby in case of issues.
* **Pros:**
    * **Instant Rollback:** If anything goes wrong with Green, flip the switch back to Blue.
    * **Zero Downtime:** The cutover is seamless.
    * **Full Testing:** Test the new version in a real production environment before users see it.
* **Cons:**
    * **Very Expensive:** Requires maintaining double the infrastructure.



---

## 3. Canary Release

* **How it works:** "Test in production" by rolling out the new version (v2) to a tiny subset of real users (e.g., 1%, 5%, or just internal employees).
    1.  Most users (e.g., 99%) still go to v1, while the small "canary" group (1%) is routed to v2.
    2.  Closely monitor this canary group for errors, performance, or bad business metrics.
    3.  If the canary is healthy, gradually increase the traffic to v2 (e.g., 10%, 50%) until all users are on the new version.
* **Pros:**
    * **Safest Rollout:** Real-world validation. Any issues affect only a small percentage of users.
    * **Fast Rollback:** If the canary shows problems, instantly roll back by routing 0% of traffic to v2.
* **Cons:**
    * **Very Complex:** Requires sophisticated monitoring ("observability").
    * **Slow Rollout:** The process is slow by design (must wait for data at each stage).
    * **Requires Advanced Routing:** Needs a load balancer smart enough to split traffic by percentage or other user attributes.


