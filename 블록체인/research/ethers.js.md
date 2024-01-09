## 질문

await window.cosmostation.ethereum.request({ method: 'eth_chainId', params: [] }) 랑

차이가 뭐지?
const ethersProvider = new ethers.providers.Web3Provider(provider);
// requestAccounts
const accounts = ethersProvider.send("eth_requestAccounts", []);

그냥 직접 리퀘스트 하기 vs 이더스 생성자에 넣어서 사용

import { useEffect, useState } from 'react';
import type { BrowserProvider } from 'ethers';
import { ethers } from 'ethers';

import { useCurrentWallet } from './useCurrentWallet';
import { ethereum } from '@cosmostation/extension-client';

function useMetaMaskProvider() {
const { currentWallet } = useCurrentWallet();
const [provider, setProvider] = useState<BrowserProvider>();

useEffect(() => {
async function connectToMetaMask() {
// Check if MetaMask is installed and enabled
if (typeof window.ethereum !== 'undefined') {
try {
if(currentWallet.walletType === 'Metamask' ) {
// Request user's permission to access their accounts
await window.ethereum.request({ method: 'eth_requestAccounts' });

          // Create an ethers.js provider using MetaMask provider
          const ethProvider = new ethers.BrowserProvider(window.ethereum);

          // Set the provider in the state
          setProvider(ethProvider);
          }
          if(currentWallet.walletType === 'Cosmostation' ) {
            const aa = await ethereum()

            // Request user's permission to access their accounts
            await window.ethereum.request({ method: 'eth_requestAccounts' });

            // Create an ethers.js provider using MetaMask provider
            const ethProvider = new ethers.BrowserProvider(aa);

            // Set the provider in the state
            setProvider(ethProvider);
            }
        } catch (error) {
          console.error('Error connecting to MetaMask:', error);
        }
      } else {
        console.error('MetaMask is not installed.');
      }
    }

    void connectToMetaMask();

}, []);

return provider;
}

export default useMetaMaskProvider;

레거시\

import { useMemo, useState } from 'react';
import { ethers } from 'ethers';
import { ethereum } from '@cosmostation/extension-client';

import type { Provider } from 'src/types/provider';

import { useCurrentWallet } from './useCurrentWallet';

export function useProvider() {
const { currentWallet } = useCurrentWallet();

const provider = useMemo(() => {
if (currentWallet.walletType === 'Metamask' && window.ethereum) {
return window.ethereum as Provider;
}

    if (currentWallet.walletType === 'Cosmostation') {
      // eslint-disable-next-line @typescript-eslint/no-unsafe-member-access
      if (window.cosmostation && window.cosmostation?.ethereum) {
        // eslint-disable-next-line @typescript-eslint/no-unsafe-member-access
        return window.cosmostation.ethereum as Provider;
      }
    }
    return undefined;

}, [currentWallet.walletType]);

const ethersProvider = useMemo(() => provider && new ethers.BrowserProvider(provider), [provider]);

return { ethersProvider };
}

useEffect(() => {
async function connectToMetaMask() {
// Check if MetaMask is installed and enabled
if (typeof window.ethereum !== 'undefined') {
try {
if (currentWallet.walletType === 'Metamask') {
// Request user's permission to access their accounts
// await window.ethereum.request({ method: 'eth_requestAccounts' });

            // Create an ethers.js provider using MetaMask provider
            const ethProvider = new ethers.BrowserProvider(window.ethereum);

            // Set the provider in the state
            setProvider(ethProvider);
          }
          if (currentWallet.walletType === 'Cosmostation') {
            const aa = await ethereum();

            // Request user's permission to access their accounts
            // await window.ethereum.request({ method: 'eth_requestAccounts' });

            // Create an ethers.js provider using MetaMask provider
            const ethProvider = new ethers.BrowserProvider(aa);

            // Set the provider in the state
            setProvider(ethProvider);
          }
        } catch (error) {
          console.error('Error connecting to MetaMask:', error);
        }
      } else {
        console.error('MetaMask is not installed.');
      }
    }

    void connectToMetaMask();

}, [currentWallet.walletType]);
