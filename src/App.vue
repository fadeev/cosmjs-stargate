<template>
  <div></div>
</template>

<script>
import { coins } from "@cosmjs/launchpad";
import { SigningStargateClient } from "@cosmjs/stargate";
import { Registry, DirectSecp256k1HdWallet } from "@cosmjs/proto-signing";
import { cosmos, google } from "./generated";
import { Type, Field } from "protobufjs";

function getTypeName(typeUrl) {
  const parts = typeUrl.split(".");
  return parts[parts.length - 1];
}

export default {
  async created() {
    //   const fromAddress = await wallet.address;
    //   const toAddress = "cosmos1nvn4sfc3r2d69c7v0j9v7ak87xepz9q5dxfv7a";

    const MsgCreatePost = new Type("MsgCreatePost")
      .add(new Field("title", 1, "string"))
      .add(new Field("body", 2, "string"))
      .add(new Field("creator", 3, "bytes"));
    const typeUrl = "/foo.foo.MsgCreatePost";
    const registry = new Registry([[typeUrl, MsgCreatePost]]);
    const msgCreatePost = MsgCreatePost.create({
      title: "foo",
      body: "bar",
    });
    const msgCreatePostBytes = MsgCreatePost.encode(msgCreatePost).finish();
    const api = "http://localhost:26657";
    const mnemonic =
      "acid design derive dirt between glance hospital soldier build tell tourist early excess primary uphold answer crisp cherry wide cannon hungry pear super retreat";
    const wallet = await DirectSecp256k1HdWallet.fromMnemonic(mnemonic);
    const stargate = SigningStargateClient;
    const client = await stargate.connectWithWallet(api, wallet, { registry });
    const fromAddress = await wallet.address;
    const msg = {
      typeUrl,
      value: {
        title: "foo1",
        body: "bar1",
        creator: "sadasda",
      },
    };
    const fee = {
      amount: coins(0, "token"),
      gas: "200000",
    };
    // const msg = {
    //   typeUrl: "/cosmos.bank.v1beta1.MsgSend",
    //   value: {
    //     amount: coins(10, "token"),
    //     fromAddress,
    //     toAddress: fromAddress,
    //   },
    // };

    console.log(await client.signAndBroadcast(fromAddress, [msg], fee));
    //   const json = {
    //     nested: {
    //       MsgCreatePost: {
    //         fields: {
    //           title: {
    //             type: "string",
    //             id: 1,
    //           },
    //           body: {
    //             type: "string",
    //             id: 2,
    //           },
    //         },
    //       },
    //     },
    //   };
    //   const root = protobuf.Root.fromJSON(json);
    //   const encoder = root.lookupType(getTypeName("/foo.foo.MsgCreatePost"));
    //   // console.log("encoder", encoder);
    //   const msgDemo = encoder.create({
    //     title: "foo",
    //     body: "bar",
    //   });
    //   // console.log(msgDemo);
    //   const msgDemoBytes = encoder.encode(msgDemo).finish();
    //   const x = google.protobuf.Any.create({
    //     type_url: "/foo.foo.MsgCreatePost",
    //     value: {
    //       title: "foo",
    //       body: "bar",
    //     },
    //   });
    //   const MsgDemoType = new protobuf.Type("MsgCreatePost")
    //     .add(new protobuf.Field("title", 1, "string"))
    //     .add(new protobuf.Field("body", 2, "string"));
    //   const registry = new Registry();
    //   registry.register("/foo.foo.MsgCreatePost", MsgDemoType);
    //   // console.log(MsgDemoType);
    //   // console.log(registry.lookupType("/cosmos.bank.v1beta1.MsgSend").type);
    //   const api = "http://localhost:26657";
    //   const mnemonic =
    //     "polar city marine oyster history ready message normal differ forward rough lazy shiver opinion setup shell unit frozen unlock embody legend actor fitness length";
    //   const wallet = await DirectSecp256k1HdWallet.fromMnemonic(mnemonic);
    //   const stargate = SigningStargateClient;
    //   const client = await stargate.connectWithWallet(api, wallet, { registry });
    //   const fromAddress = await wallet.address;
    //   const toAddress = "cosmos1nvn4sfc3r2d69c7v0j9v7ak87xepz9q5dxfv7a";
    //   const denom = "token";
    //   const msg = {
    //     typeUrl: "/cosmos.bank.v1beta1.MsgSend",
    //     value: {
    //       amount: coins(10, denom),
    //       fromAddress,
    //       toAddress,
    //     },
    //   };
    //   const fee = {
    //     amount: coins(0, denom),
    //     gas: "200000",
    //   };
    // console.log("registry", registry);
    // console.log(await client.signAndBroadcast(fromAddress, [x], fee));
  },
};
</script>
