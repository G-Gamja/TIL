```tsx

import MetaMaskProvider from "@/providers/MetamaskProvider";
import { useSDK } from "@metamask/sdk-react";

export const ConnectWalletButton = () => {
  const [account, setAccount] = useState<string | undefined>();

  const { sdk, connected, connecting } = useSDK();

  const connect = async () => {
    try {
      const metaMaskAccount = (await sdk?.connect()) as string[];
      setAccount(metaMaskAccount[0]);
    } catch (err) {
      console.warn(`No accounts found`, err);
    }
  };

  const disconnect = () => {
    if (sdk) {
      sdk.terminate();
      setAccount(undefined); // Optionally reset the account state
    }
  };
```
