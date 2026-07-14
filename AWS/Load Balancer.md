# AWS Load Balancer

## What is it?
- A Load Balancer **distributes incoming traffic** across multiple servers
- Prevents any single server from being overwhelmed
- Think of it like a **traffic cop** at an intersection directing cars to different lanes

## Why Use It?
- **High availability**: If one server crashes, traffic goes to others
- **Scalability**: Add more servers and load balancer handles it
- **SSL termination**: Handle HTTPS at the load balancer level

## Types in AWS
| Type | Use Case |
|------|----------|
| **ALB** (Application Load Balancer) | HTTP/HTTPS traffic, path-based routing |
| **NLB** (Network Load Balancer) | TCP/UDP, ultra-low latency |
| **CLB** (Classic Load Balancer) | Legacy, avoid for new projects |

## ALB Key Features
- Route `/api/*` to one group of servers and `/static/*` to another
- **Health checks**: Automatically removes unhealthy instances
- Supports **WebSockets**
- Works with **Auto Scaling Groups**

## How It Works
```
User Request
     ↓
[Load Balancer]
     ↓         ↓         ↓
 [Server 1] [Server 2] [Server 3]
```

## Key Terms
- **Target Group**: A group of servers the LB routes to
- **Listener**: The port/protocol the LB listens on (e.g., port 443 for HTTPS)
- **Health Check**: LB pings each server — unhealthy ones stop receiving traffic
- **Sticky Sessions**: Same user always goes to the same server (useful for sessions)

## Good to Know
- ALB can route based on **URL path**, **host**, or **HTTP headers**
- Use with **Route 53** (AWS DNS) for custom domain names
- Free tier includes 750 hours/month of Classic LB
