# encdec
Encryption &amp; Decryption package for golang


func main() {

	startingTime := time.Now()

	privKey, pubKey := GenerateRsaKeyPair()
	fmt.Println("PrivateKey:", privKey)
	pkey := ExportRsaPrivateKeyAsPemStr(privKey)

	privKey.Precompute()
	err := privKey.Validate()
	if err != nil {
		fmt.Println("Error:", err.Error())
		os.Exit(1)
	}

	fmt.Println("Key-1:", pkey)
	fmt.Println("-----------------")

	pkey2, err := ParseRsaPrivateKeyFromPemStr(pkey)
	fmt.Println(err, "Key-2:", pkey2)

	pubKeystr, _ := ExportRsaPublicKeyAsPemStr(pubKey)

	pubKey2, err := ParseRsaPublicKeyFromPemStr(pubKeystr)

	fmt.Println("PublicKey:", pubKeystr, err)

	fmt.Println(pubKey, err)
	fmt.Println("*************************")
	fmt.Println(pubKey2)

	var label []byte
	sourceText := []byte(`I Love my country`)
	encryptedText := encrypt(pubKey, sourceText, label)
	fmt.Println("encryptedText:", encryptedText)

	fmt.Println("#################")
	fmt.Println("sourceText...:", sourceText)
	decryptedText := decrypt(pkey2, encryptedText, label)
	fmt.Println("decryptedText:", decryptedText)

	fmt.Println("TimeTaken:", time.Since(startingTime).Milliseconds(), " ms")

}
