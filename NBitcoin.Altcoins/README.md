# NBitcoin.Altcoins

Currently supported altcoins are:

* Litecoin
* BCash (also known as Bitcoin Cash)

## How to use?

This package expose altcoin's `Network` class.
For example if you want to use Litecoin testnet:

Once in your program call:

```
NBitcoin.Altcoins.Litecoin.EnsureRegistered();
```

Then you can get the network instance in this way:

```
Network network = NBitcoin.Altcoins.Litecoin.Testnet;
```

## How to support my own altcoin?

Follow Litecoin example and make a pull request.

NBitcoin developers do not test those PRs, so you are responsible to keep it working.

## How to test?

If you wnat to test your newly created `Network`, update [WellknownNodeDownloadData](../NBitcoin.Tests/WellknownNodeDownloadData.cs) so the test environment can download binaries and run for your blockchain on regtest.

Then, change [NodeBuilderEx](../NBitcoin.Tests/NodeBuilderEx.cs) like the following example.

```
public static NodeBuilder Create([CallerMemberName] string caller = null)
{
	Altcoins.Litecoin.EnsureRegistered();
	return NodeBuilder.Create(NodeDownloadData.Litecoin.v0_15_1, Altcoins.Litecoin.Regtest, caller);
}
```

You can then run the tests for your altcoin in command line from the NBitcoin.Tests project:

Note that the first time can take a while because the test environment download the node binaries.

```
dotnet test -c Release NBitcoin.Tests.csproj --filter "Altcoins=Altcoins" -p:ParallelizeTestCollections=false --framework netcoreapp2.0
```

You can also manually execute any test with Visual Studio.

Note that the tests with the trait `Altcoins=Altcoins` are only doing some sanity check. You might want to run additional tests.
