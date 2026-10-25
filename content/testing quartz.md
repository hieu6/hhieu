```bash
npx quartz build --serve
```

- Quartz uses local address `http://localhost:8080`. 
- Issue: build succeeds, site loads, but looks styling and layout is broken. 
- Error: "Either this page is private or doesn't exist" is Quartz's own 404 page, not a styling error. 
- Next: check which pages were generated and whether browser is requesting the correct path.

`ls public`
