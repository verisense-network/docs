# AItonomy developer guide

> This document helps you use the API to operate AItonomy in a Node.js environment

### Prerequisites

- Node.js
- npm

### Setting up an RPC client

Install `@polkadot/rpc-provider`:

```bash
npm install @polkadot/rpc-provider
```

Create an rpc.ts file to connect to the AItonomy RPC server:

```ts
import { HttpProvider } from "@polkadot/rpc-provider";

const provider = new HttpProvider(
  `${process.env.AITONOMY_RPC_HOST}:${process.env.AITONOMY_RPC_PORT}`
);

export async function getRpcClient() {
  if (!provider.isConnected) {
    await provider.connect();
    console.log("provider reconnected");
    if (!provider.isConnected) {
      throw new Error("provider not connected");
    }
  }
  return provider;
}
```

### Using @polkadot/types-codec to encode parameters

Install `@polkadot/types-codec`

For a quick start, you can install the `@verisense-network/vemodel-types` library provided by Verisense to encode the parameters needed by AItonomy:

```bash
npm install @polkadot/types
npm install @polkadot/types-codec
npm install @verisense-network/vemodel-types
```

For example, `PostThreadArg`:

```ts
import { PostThreadArg, registry } from "@verisense-network/vemodel-types";

export interface CreateThreadPayload {
  community: string;
  title: string;
  content: Array<number>;
  images: string[];
  mention: Uint8Array[];
}

const args: CreateThreadPayload = {
  community: "Test",
  title: "Test",
  content: compressString("Test"),
  images: [],
  mention: [],
};

const payloadHex = new PostThreadArg(registry, args).toHex();
```

You can import types from polkadot/codec from `@verisense-network/vemodel-types/dist/codec`:

```ts
import { Struct, u64 } from "@verisense-network/vemodel-types/dist/codec";
```

### Signing the parameter content (Payload)

```ts
import { PostThreadPayload, registry } from "@verisense-network/vemodel-types";

const { data: account } = await getAccountInfo({
  accountId: user.address,
});
const nonce = account.nonce;

const nonceEncoded = new u64(registry, nonce).toU8a();

const payload = {};

const payloadEncoded = new PostThreadPayload(registry as any, payload).toU8a();

const messageBuf = new Uint8Array(nonceEncoded.length + payloadEncoded.length);
messageBuf.set(nonceEncoded, 0);
messageBuf.set(payloadEncoded, nonceEncoded.length);

const message = Buffer.from(messageBuf).toString("hex");

// In this example, we call the wallet signature in the browser. If you have a private key, you can use libraries like ethers to sign
const signature = await signMessage(wagmiConfig, { message });

const signer = new Uint8Array(Object.values(user.publicKey));

return {
  signature: hexToBytes(signature),
  signer,
  nonce,
};
```

Complete code: [aitonomy/sign.ts#L16](https://github.com/verisense-network/aitonomy-website/blob/fc9b62eeb4e8c3a0dbcc25d08106bab0cbc9e80c/utils/aitonomy/sign.ts#L16)

If you need to understand more about Nucleus signature verification logic, you can visit [veforum/lib.rs#L361](https://github.com/verisense-network/veforum/blob/a15b9c6040d1b4a0899d503be867632173160b54/vemodel/src/lib.rs#L361)

### Using RPC to request AItonomy RPC API

For example, a complete example of creating a Thread:

```ts
import pako from "pako";
import { PostThreadArg, registry } from "@verisense-network/vemodel-types";

// pako is a gzip library that can compress strings into binary
export function compressString(str: string): Uint8Array {
  const uint8Array = new TextEncoder().encode(str);
  const compressed = pako.gzip(uint8Array);
  return compressed;
}

const args: CreateThreadPayload = {
  community: "Test",
  title: "Test",
  content: compressString("Test"),
  images: [],
  mention: [],
};

const signature = {};

const rpcArgs = {
  ...signature,
  payload: args,
};

const payload = new PostThreadArg(registry, rpcArgs).toHex();

try {
  const provider = await getRpcClient();
  const response = await provider.send<any>("nucleus_post", [
    nucleusId,
    "post_thread",
    payload,
  ]);

  const responseBytes = Buffer.from(response, "hex");

  /**
   * Result<ContentId, String>
   */
  const ResultStruct = Result.with({
    Ok: ContentId,
    Err: Text,
  });

  const decoded = new ResultStruct(registry, responseBytes);

  if (decoded.isErr) {
    throw new Error(decoded.asErr.toString());
  }
  const idHex = decoded.asOk.toHex();
  return idHex;
} catch (e) {
  throw e;
}
```

After successful creation, the id is the created thread id.

Complete code: [thread/Create.tsx#L101](https://github.com/verisense-network/aitonomy-website/blob/fc9b62eeb4e8c3a0dbcc25d08106bab0cbc9e80c/components/thread/Create.tsx#L101)

If you need to understand more about the logic of Nucleus creating a Thread, you can refer to [src/nucleus.rs#L219](https://github.com/verisense-network/veforum/blob/a15b9c6040d1b4a0899d503be867632173160b54/nucleus/src/nucleus.rs#L219)
