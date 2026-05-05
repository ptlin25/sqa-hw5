Performance test: JSONPlaceholder Posts/Comments feature

Target: https://jsonplaceholder.typicode.com

Feature under test: Post browsing and comment retrieval

## Operational Profile
browse all posts -> read a single post -> view post comments or view author profile

Users start on the landing page and browse all posts. When they see a post that interests them, they will click on the post to read more. From there, they might view the comments or view the author's profile.

## Load Profile
1. Browse all posts: 40% users
2. Read a single post: 30% users
3. View post comments: 20% users
4. View author profile: 10% users

The load test will ramp up to 100 VUs over a minute, hold for 3 minutes, and then ramp down. The spike test will ramp up to 100 VUs over a minute, spike to 1000 VUs in 10 seconds, hold for a minute, drop to 100 VUs in 10 seconds, and then ramp down.


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