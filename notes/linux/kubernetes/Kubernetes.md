Learning from [Udemy](https://www.udemy.com/course/kubernetes-masterclass-for-beginners/?couponCode=SEPTSTACK24A) course by Mischa van der Burg.

Four ways you can access a pod/container in Kubernetes/k8s:

1. Ingress
2. Port-forwarding
3. NodePort (though with this you gotta be mindful that not all managed k8s services provide the cluster in a public subnet. For instance, AWS EKS, using the [Terraform config](https://developer.hashicorp.com/terraform/tutorials/kubernetes/eks), puts the clusters in a private subnet. Therefore, NodePort will not work in this case.)
4. LoadBalancer.

Learn [[Helm]], Kubernetes package manager.
