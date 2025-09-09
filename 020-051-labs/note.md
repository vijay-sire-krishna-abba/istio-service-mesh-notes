
```
$ curl -L https://istio.io/downloadIstio | sh -
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   101  100   101    0     0    699      0 --:--:-- --:--:-- --:--:--   701
100  5124  100  5124    0     0  19298      0 --:--:-- --:--:-- --:--:-- 19298

Downloading istio-1.27.0 from https://github.com/istio/istio/releases/download/1.27.0/istio-1.27.0-linux-amd64.tar.gz ...

Istio 1.27.0 download complete!

The Istio release archive has been downloaded to the istio-1.27.0 directory.

To configure the istioctl client tool for your workstation,
add the /root/istio-1.27.0/bin directory to your environment path variable with:
         export PATH="$PATH:/root/istio-1.27.0/bin"

Begin the Istio pre-installation check by running:
         istioctl x precheck 

Try Istio in ambient mode
        https://istio.io/latest/docs/ambient/getting-started/
Try Istio in sidecar mode
        https://istio.io/latest/docs/setup/getting-started/
Install guides for ambient mode
        https://istio.io/latest/docs/ambient/install/
Install guides for sidecar mode
        https://istio.io/latest/docs/setup/install/

Need more information? Visit https://istio.io/latest/docs/ 

```


```
$ cd istio-1.27.0/

$ ls
bin  LICENSE  manifests  manifest.yaml  README.md  samples  tools

$ export PATH=$PWD/bin:$PATH
```


```
$ istioctl install -f samples/bookinfo/demo-profile-no-gateways.yaml -y
        |\          
        | \         
        |  \        
        |   \       
      /||    \      
     / ||     \     
    /  ||      \    
   /   ||       \   
  /    ||        \  
 /     ||         \ 
/______||__________\
____________________
  \__       _____/  
     \_____/        

✔ Istio core installed ⛵️                                                                                                                                                         
✔ Istiod installed 🧠                                                                                                                                                             
✔ Installation complete                                                                                                                                                          
```

```
$ istioctl install --set profile=demo -y
        |\          
        | \         
        |  \        
        |   \       
      /||    \      
     / ||     \     
    /  ||      \    
   /   ||       \   
  /    ||        \  
 /     ||         \ 
/______||__________\
____________________
  \__       _____/  
     \_____/        

✔ Istio core installed ⛵️                                                                                                                                                         
✔ Istiod installed 🧠                                                                                                                                                             
✔ Egress gateways installed 🛫                                                                                                                                                    
✔ Ingress gateways installed 🛬                                                                                                                                                  
✔ Installation complete                                                                                                                                                          

$ istioctl version 
client version: 1.27.0
control plane version: 1.27.0
data plane version: 1.27.0 (2 proxies)

```