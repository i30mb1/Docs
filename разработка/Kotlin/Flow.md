### ChannelFlow
allows developers to produce data asynchronously from different sources
```Kotlin
val nearbyDevices = channelFlow {  
launch {  
	send(LANHelper.discoverConnectableHosts())  
	}  
launch {  
	send(BluetoothHelper.discoverConnectableDevices())  
	}  
}
```

it can 
```Kotlin
fun networkDataFlow() = flow {  
emit(LANHelper.discoverConnectableHosts())  
}  
  
fun bluetoothDiscoveryFlow() = flow {  
emit(BluetoothHelper.discoverConnectableDevices())  
}  
  
merge(  
networkDataFlow(),  
bluetoothDiscoveryFlow()
) { networkData, bluetoothData ->  
	// React to the data  
}
```