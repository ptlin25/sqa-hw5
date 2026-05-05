Performance test: JSONPlaceholder Posts/Comments feature

Target: https://jsonplaceholder.typicode.com

Feature under test: Post browsing and comment retrieval

## Operational Profile
browse all posts -> read a single post -> view post comments or view author profile

1. Browse all posts: 40% users
2. Read a single post: 30% users
3. View post comments: 20% users
4. View author profile: 10% users

The operational profile models a typical blog reader session on JSONPlaceholder. 
Users naturally start by browsing all posts, so it has the highest weight. Some 
users will click into a specific post, a smaller portion will read comments, and
an even smaller portion will read the author's profile (10%). 

## Load Profile
The load test ramps up to 100 VUs over one minute, holds for three minutes, then
ramps down — this simulates sustained normal traffic and validates that the 
system can meet SLAs under normal conditions. The spike test starts at a 100 VU 
baseline, then surges to 1,000 VUs in 10 seconds to simulate a sudden burst of 
traffic, then recovers back to baseline to observe how the system stabilizes.


## SLAs (pass/fail thresholds — evaluated after each test run)
- p95 response time: < 100 ms
- Error rate: < 1%
- Throughput: > 25 RPS

## Test types (selected via TEST_TYPE env var, default = "load")
1. load  — gradual ramp 0→50 VUs, 3-minute steady state, then ramp-down. Validates normal-traffic SLAs and baseline capacity.

2. spike — baseline 10 VUs, instant spike to 100 VUs for 90 s, recovery. Validates resilience to sudden viral / flash-sale traffic bursts.

## Locust Setup
```
pip3 install locust
```

## Run commands
```
# load test
TEST_TYPE=load locust

# spike test
TEST_TYPE=spike locust
```