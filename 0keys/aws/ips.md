# nodeswarm01
ssh -i "nodeswarm01.pem" ec2-user@ec2-18-207-128-245.compute-1.amazonaws.com
18.207.128.245
docker swarm join --token SWMTKN-1-35zt9anonsb377rrgsx0ym03g3g7ie72fm2j2v1pg9ktawmrgt-8uyc6bcvqwkaamu4sj3gfy9cz 172.31.20.21:2377

# nodeswarm02
ssh -i "nodeswarm01.pem" ec2-user@ec2-50-17-13-220.compute-1.amazonaws.com
50.17.13.220

# nodeswarm03
ssh -i "nodeswarm01.pem" ec2-user@ec2-54-236-212-212.compute-1.amazonaws.com
54.236.212.212