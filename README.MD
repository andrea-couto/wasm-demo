[ ] Get index.html from golang repo

[ ] Change index.html to say main.wasm

[ ] Get wasm_exec.js from golang repo

[ ] Make main.go hello world

[ ] Set GOOS=js

[ ] Set GOARCH=wasm

[ ] go build -o main.wasm main.go

[ ] Make a web server github.com/andrea-couto/golang-wasm-demo

[ ] Build and run the server


    In order to pass values from go to javascript we need syscall/js package
    HOWEVER, goland is prohibiting us from using syscall/js until we set default
    OS and ARCH to JS, WASM


[ ] Open up syscall/js under external libraries

[ ] Click "Edit go project settings"

[ ] OS = js ARCH=wasm

[ ] Make a channel and JS callback in main

	c := make(chan struct{},0)
	js.Global().Set("encrypt", js.NewCallback(encrypt))
	<-c

[ ]  Create place to take in input

    <textarea id="input" placeholder="Enter your plaintext"></textarea><br>

[ ] Create a place to put ciphertext (AKA output)

    <div id="output"></div><br>

[ ] Create a convert function

    function convert() {
        let input = document.querySelector('#input').value;
        encrypt(input, output);
    }

[ ] change button to be convert() and removed disabled

[ ] add run(); to new Go()

[ ] Create a output function to get the innerHtml


    function output(ciphertext) {
        document.querySelector('#output').innerHTML = ciphertext;\
    }


[ ] make an encrypt function


    func encrypt (input []js.Value) {
        PrivateKey, _ := rsa.GenerateKey(rand.Reader, 1000)
        PublicKey := &PrivateKey.PublicKey
        message := input[0].String()
        byteMessage := []byte(message)
        label := []byte("")
        hash := sha256.New()
        ciphertext, _ := rsa.EncryptOAEP(hash, rand.Reader, PublicKey, byteMessage, label)
        input[1].Invoke(fmt.Sprintf("[%x]", ciphertext))
     }


[ ] rebuild the wasm file

``` go build -o main.wasm main.go```

[ ] rerun the server

[ ] can you print or display the public key?

