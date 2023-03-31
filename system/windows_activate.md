A Multiple Activation Key (MAK) is used for one-time activation of systems through Microsoft's hosted activation services. This requires a connection with a Microsoft activation server. Once computers are activated using a MAK key, no further communication with Microsoft is required .

MAK keys are used to activate a specific number of devices. The count is pre-configured as a deal between Microsoft and Enterprise. Every time a device is activated using MAK this is what happens: The connection is established to Microsoft’s own activation service. The key is verified i.e. validated if any more copies can be activated using that key. If that’s a yes, 1 is subtracted from the number of activations still available .




Yes, Windows Server Datacenter can be activated using Key Management Service (KMS) . To use KMS, you need to have a KMS host available on your local network. The computers that are activated using a KMS host need to have a specific product key. This key is sometimes called a KMS client key, but its official name is Microsoft Generic Volume License Key (GVLK) .

By default, computers running volume license editions of Windows Server and Windows client are KMS clients with no additional configuration needed because the relevant GVLK is already present. However, in some cases, you may need to add the GVLK to the computer you want to activate against a KMS host .
