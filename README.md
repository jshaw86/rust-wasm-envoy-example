# Setup
1. Install rustup https://www.rust-lang.org/tools/install
2. Install wasm target 
```
rustup target add wasm32-wasi
```
3. Install Kind
```
brew install kind
```
4. Install istioctl
```
curl -L https://istio.io/downloadIstio | ISTIO_VERSION=1.10.4 TARGET_ARCH=x86_64 sh -
cd istio-1.10.4
export PATH=$PWD/bin:$PATH
```
5. Create istio cluster
```
kind create cluster --name istio-testing
kubectl config use-context kind-istio-testing
```
6. Install istio
```
istioctl install --set profile=demo -y
```
7. Setup demo app
```
kubectl apply -f https://raw.githubusercontent.com/istio/istio/master/samples/bookinfo/platform/kube/bookinfo.yaml
```
8. Test setup
```
kubectl exec "$(kubectl get pod -l app=ratings -o jsonpath='{.items[0].metadata.name}')" -c ratings -- curl -sS productpage:9080/productpage | grep -o "<title>.*</title>"
```

# Build

1. Build and copy wasm binary to root directory
```
cargo build --target wasm32-wasi --release
```


