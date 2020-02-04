
# Usage

```go
cluster := "https://mycluster.region.kusto.windows.net"
appId := ""
appKey := ""
tenantId := ""
kustoContext, err := azkustodata.AuthenticateWithAadApp(cluster, appId, appKey, tenantId)

if (err != nil) {
    panic(err)
}

client := azkustodata.NewKustoClient(*kustoContext)

db := "SampleDB"
query := "SampleTable | count"

iter, err := kustoClient.Query(ctx, db, query)
if err != nil {
    panic(err)
}

defer iter.Stop()



// Loop through the iterated results, read them into our UserID structs and append them
// to our list of recs.
var recs []CountResult
for {
    row, err := iter.Next()
    if err != nil {
        // This indicates we are done.
        if err == io.EOF {
            break
        }
        // We ran into an error during the stream.
        panic(err)
    }
    rec := CountResult{}
    if err := row.ToStruct(&rec); err != nil {
        panic(err)
    }
    recs = append(recs, rec)
}

print(recs)
```

# Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.opensource.microsoft.com.

When you submit a pull request, a CLA bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., status check, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.
