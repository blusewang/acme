# acme
ACME for ZeroSSL

DEMO：

```cgo
	raw, _ := base64.RawURLEncoding.DecodeString(EAB_HMAC_KEY)
	var cm = &autocert.Manager{
		Prompt:     autocert.AcceptTOS,
		HostPolicy: autocert.HostWhitelist("a.domain.com","b.domain.com"),
		Cache:      autocert.DirCache("/tmp"),
		Email:      "your@mail.com",
		ExternalAccountBinding: &acme.ExternalAccountBinding{
			KID: EAB_KID,
			Key: raw,
		},
	}
	httpsServer = &http.Server{
		Addr:      ":https",
		TLSConfig: cm.TLSConfig(),
		Handler:   handler,
	}
	go httpsServer.ListenAndServeTLS("", "")

	httpServer = &http.Server{
		Addr:    ":http",
		Handler: cm.HTTPHandler(handler),
	}
	go httpServer.ListenAndServe()
```