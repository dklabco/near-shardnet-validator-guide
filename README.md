# near-shardnet-validator-guide

Part 1: Creating a new server instance & log in securely with the best practices
===

I started by registering a new hosting account from one of the well known providers. I already have a node running on Hetzner, for the purpose of this guide, I create another one with Digital Ocean.

![Screen Shot 2022-07-29 at 00 27 42__SW1](https://user-images.githubusercontent.com/2597375/188728819-2d232514-61bf-4bc3-9e47-e61b9b0873ef.png)
![Screen Shot 2022-07-29 at 00 29 01__SW2](https://user-images.githubusercontent.com/2597375/188728958-357c1f3d-83cb-4a7e-8310-5ac64378ad8d.png)
![Screen Shot 2022-07-29 at 00 36 57__SW3](https://user-images.githubusercontent.com/2597375/188729468-77a6ab8d-c00b-49c9-9f36-d1f24bcb7e70.png)

Note that, when creating a brand new Digital Ocean account, with a new payment method, we're entitled to $100 credit to kick start our project. Nice, itsn't it?

Each server instance is called a "droplet" in Digital Ocean terminology, so here I'm creating a new droplet. Please refer to the [recommended hardware](https://github.com/near/stakewars-iii/blob/main/challenges/002.md#server-requirements) guide and choose the server specs that are best to run a NEAR network validator node.

![Screen Shot 2022-07-29 at 00 43 26__SW4](https://user-images.githubusercontent.com/2597375/188731172-938b6086-d814-4288-bd54-251c06034045.png)

Note that, in the screenshot above I chose the "$48/month" option with "8 GB / 4 CPU", but later on I realized it was not enough. Probably the cloud vCPU is not as powerful as a real CPU? Either way, if possible, you're recommended to choose a spec higher than shown in the screenshot above.

During the server provisioning process, I deliberately set up an ssh key authorization, so that later I could securely log into my server without any password prompt. This step is optional, but I always prefer to have it for all my servers.

![Screen Shot 2022-07-29 at 00 55 35__SW5](https://user-images.githubusercontent.com/2597375/188731261-918480bf-a2f5-4012-8c24-475a1a750504.png)
![Screen Shot 2022-07-29 at 00 57 47__SW6](https://user-images.githubusercontent.com/2597375/188731306-5529660d-6d42-4d86-8a83-b9a1d9621e7d.png)

As shown above, I simply generated a new key pair on my local computer (local worksation), then add the public key to the server. Digital Ocean provides an interface for us to do this. If you use other cloud providers, there should be similar interfaces for this process.

Once all setup is filled in, I double check everything one last time before confirming to create the server (aka "droplet"):

![Screen Shot 2022-07-29 at 01 00 11__SW7](https://user-images.githubusercontent.com/2597375/188731366-d26ac6d5-da69-4fa7-9eb7-b06b1b39f382.png)
![Screen Shot 2022-07-29 at 01 01 13__SW8](https://user-images.githubusercontent.com/2597375/188731414-76097b95-9d0d-4359-aba5-691ffca6ddf5.png)
![Screen Shot 2022-07-29 at 01 02 50__SW9](https://user-images.githubusercontent.com/2597375/188731459-4554ecb2-308b-4194-98f8-68755c9869c7.png)

Now I just need to log into the server with the correct key. The first time logging in, you'll be prompted e.g. "are you sure…." - so go ahead and proceed. There won't be any prompt during subsequent logins.

![Screen Shot 2022-07-29 at 01 10 53__SW10](https://user-images.githubusercontent.com/2597375/188731570-09680c0f-6970-486a-a7fc-0f47af730925.png)

It's a good practice to NOT operate as the `root` user. Root users have too much power, and a small mistake can easily render your server less secured, or completely broken. So, the 1st thing we do after successfully log in as `root`, is to create a non-root (normal) user. Here's an example of how one might do this:

![Screen Shot 2022-07-29 at 01 43 13__SW11](https://user-images.githubusercontent.com/2597375/188731902-2ea5b8c9-d219-492b-b493-7f4dd846c496.png)

Once you've got a normal user created, please feel free to set up the ssh login for that user (just like you did earlier with the `root` user). The only difference is that this time you don't have the Digital Ocean interface for this. How to do that is beyond the scope of this document, but here's an example tutorial. Just like last time, this step is optional. If you do it correctly, you'll be able to log into the server as the non-root user, like shown in this example screenshot:

![Screen Shot 2022-07-29 at 01 45 46__SW12](https://user-images.githubusercontent.com/2597375/188731950-84514c77-b708-4e43-8dd7-7404c87a2e86.png)

------

[Part 2: Installing tools on the local machine](https://github.com/near/stakewars-iii/blob/main/challenges/001.md#setup-near-cli)
===

All tools like `node`, `npm`, `near-cli` can be installed exactly as instructed in the Stakewars guide. Below, however, is how I typically install the tools for myself. Note that you don't have to do exactly as shown below. As long as you have all the tools at the end of the day, then you're good.

### installing nvm (this helps us manage all different versions of Node.js on the same host machine)
![Screen Shot 2022-07-29 at 01 48 53__SW13](https://user-images.githubusercontent.com/2597375/188731982-ff2a8630-ed48-4624-aed0-cc36e9539ea1.png)
### installing Node.js (via the help of `nvm` above)
![Screen Shot 2022-07-29 at 01 49 55__SW14](https://user-images.githubusercontent.com/2597375/188732027-64b0cd90-0fc8-4b91-836a-a01dcd5c94ab.png)
### installing `near-cli` - the command-line interface for the NEAR ecosystem
![Screen Shot 2022-07-29 at 01 51 56__SW15](https://user-images.githubusercontent.com/2597375/188732053-41feb3ea-779f-4eab-a344-47c8ab59e004.png)
![Screen Shot 2022-07-29 at 01 52 57__SW16](https://user-images.githubusercontent.com/2597375/188732078-c84981ef-c99b-474e-b302-cdfd3fb4dc92.png)


Note that above, we explicitly set `NEAR_ENV` to `shardnet` for all login sessions. If you intend to use this user, on this server, for only shardnet (for the purpose of the Stakewars), then you should do this. Alternatively, you don't have to do this, but you must set `NEAR_ENV` for each and every `near-cli` command you invoke, otherwise it'll default to `testnet`, which is completely different from `shardnet`.

With `near-cli` installed, we can now investigate the blockchain network info, such as the validator proposals:
![Screen Shot 2022-07-29 at 01 54 33__SW17](https://user-images.githubusercontent.com/2597375/188732103-da57f73b-3203-41c5-8198-a2cf166c96ad.png)

or the performance of current validators:
![Screen Shot 2022-07-29 at 01 54 58__SW18](https://user-images.githubusercontent.com/2597375/188732124-4d26c29e-e229-4e21-abcf-ab1ee88f125a.png)

or the state of the validator nodes in the near future:
![Screen Shot 2022-07-29 at 01 55 51__SW19](https://user-images.githubusercontent.com/2597375/188732142-c9904421-4ede-4a7f-9d99-ad9d35cbe8c8.png)

---

[Part 3: Installing system tools on the server machine](https://github.com/near/stakewars-iii/blob/main/challenges/002.md#prerequisites)
===

Heads up: earlier, we created a non-root user to handle non-system operations (e.g. interacting with the NEAR node). However, the next step requires us to be logged in as root (since we're installing core system dependencies).

First, as instructed, we'll verify that the server CPU architecture supports NEAR node:
![Screen Shot 2022-07-29 at 02 01 45__SW20](https://user-images.githubusercontent.com/2597375/188732236-6668faa0-5dd0-4a76-95b0-2244940f364d.png)

Next, all system dependencies are to be installed:
![Screen Shot 2022-07-29 at 02 04 04__SW21](https://user-images.githubusercontent.com/2597375/188732257-a1d9c9db-9a51-400a-b970-c7c8368be5f3.png)

When I first did this, the Stakewars guide had a typo, and as a result, `python3-pip` was not installed on my server. Therefore, I needed to install it explicitly afterwards:
![Screen Shot 2022-07-29 at 02 05 39__SW22](https://user-images.githubusercontent.com/2597375/188732292-5bf8da15-f3a6-4c9d-9d50-81a186fd4291.png)

Again, just follow the official guideline and set up the rest of the dependencies & system env vars:
![Screen Shot 2022-07-29 at 02 07 08__SW23](https://user-images.githubusercontent.com/2597375/188732334-58fdccbf-5ca4-4eee-9c00-ab230d877aab.png)
![Screen Shot 2022-07-29 at 02 08 57__SW24](https://user-images.githubusercontent.com/2597375/188732353-505104a8-1e97-4f87-93d6-3af652d778fb.png)


Next up is to install the `rust` toolchain. To my knowledge, `rust` itself can be installed per-user, so for this I can log in as a non-root user. Your mileage here might vary, but in my case, I got a small hiccup, as the system `rust` was interfering with the user `rust`. This was how the hiccup was:

![Screen Shot 2022-07-29 at 02 10 22__SW25](https://user-images.githubusercontent.com/2597375/188732373-c3ea7401-158f-43ef-8f75-d1797340bcb6.png)

To resolve the issue, I had to log in as root again, and remove the `cargo` and `rustc` packages from the system:
![Screen Shot 2022-07-29 at 02 11 03__SW26](https://user-images.githubusercontent.com/2597375/188732405-367de165-44c3-4bfd-866e-6b0870e351d5.png)
![Screen Shot 2022-07-29 at 02 14 21__SW27](https://user-images.githubusercontent.com/2597375/188732437-ba234f61-dd89-4118-a596-7cd97fd6f499.png)


Once those are done, as the non-root user, I can finally install `rust`:
![Screen Shot 2022-07-29 at 02 14 54__SW28](https://user-images.githubusercontent.com/2597375/188732466-d9db50f0-7a12-4651-80a0-367def610963.png)
![Screen Shot 2022-07-29 at 02 16 21__SW29](https://user-images.githubusercontent.com/2597375/188732494-b51a1bd0-dca8-49fd-91ec-5e1350bd0b28.png)

With `rust` installed, let's clone the NEAR sourcecode and compile `nearcore` ourselves!
![Screen Shot 2022-07-29 at 02 17 56__SW30](https://user-images.githubusercontent.com/2597375/188732829-31c86887-c7e8-4440-998e-8cc606f08665.png)
![Screen Shot 2022-07-29 at 02 19 00__SW31](https://user-images.githubusercontent.com/2597375/188732890-3e598517-667e-40ca-a16d-88834ba0de87.png)
![Screen Shot 2022-07-29 at 02 19 48__SW32](https://user-images.githubusercontent.com/2597375/188733003-18971816-641d-484b-83ac-472a481011b7.png)
![Screen Shot 2022-07-29 at 02 53 49__SW33](https://user-images.githubusercontent.com/2597375/188733065-f7d6df33-3196-4b94-8daf-625f0ab1bb0d.png)

Once `nearcore` is compiled, we'll pull the system configuration files as needed (e.g. `genesis.json`, `config.json` and the other node specific config files):
![Screen Shot 2022-07-29 at 02 58 45__SW34](https://user-images.githubusercontent.com/2597375/188733123-5f87d73c-36a1-4f07-952e-db3ce82bb693.png)

As shown above, I saved the original `genesis.json` file into `genesis.json.original`, then download the new genesis (as instructed by the Stakewars guide) as my official `genesis.json` file. Again, you don't have to do exactly as I did; as long as you have the correct `genesis.json` file, then you're good.

With the correct genesis file in place, we can actually run the node and start participating in the chain (although not as a validator node - yet)!
![Screen Shot 2022-07-29 at 03 01 34__SW35](https://user-images.githubusercontent.com/2597375/188733464-150b409e-9a6a-44ef-8f73-04b152f28dd7.png)

I'd suggest that you leave the node running (like in the screenshot above), as it'll take some time for it to fully sync anyways. The plan is, while it syncs, we'll take care of something else, then later when it's fully synced, we'll finish the rest of the server-side setup.

------

[Part 4: Generate a validator key](https://github.com/near/stakewars-iii/blob/main/challenges/002.md#activating-the-node-as-validator)
===

1. NEAR validator key implements the public-private key mechanism. Using the `near generate-key` command, a file will be written to a designated destination, that file contains the key pair that can then be used as your node "validator key".
2. The key generation operation can happen anywhere (on any computer, not necessarily the server that hosts your NEAR validator node), so it's recommended that you generate it on your local computer, then copy the file over to the server where nearcore runs.

HEADS UP: in the screenshot below, I can call `near-cli` commands without specifying `NEAR_ENV=shardnet` because I already set it as a default env var for my session (on my local machine). If you don't do so, you'll need to specify the env var for each and every command.

First, you'll need to authenticate yourself with the NEAR blockchain. Executing `near login` should trigger this process:
![Screen Shot 2022-07-29 at 03 02 44__SW36](https://user-images.githubusercontent.com/2597375/188733523-4c0a757a-c069-49b1-9edd-8aa6a7448d75.png)
![Screen Shot 2022-07-29 at 03 03 35__SW36b](https://user-images.githubusercontent.com/2597375/188733565-26721d57-2b88-4b8e-a7eb-df074f6a367f.png)
![Screen Shot 2022-07-29 at 03 03 58__SW36c](https://user-images.githubusercontent.com/2597375/188733606-3612e54c-54f5-4c8c-aff6-42f0d35380df.png)
![Screen Shot 2022-07-29 at 03 04 39__SW36d](https://user-images.githubusercontent.com/2597375/188733630-59fe941f-977b-47f4-91a5-a492e428bf04.png)
![Screen Shot 2022-07-29 at 03 05 03__SW36e](https://user-images.githubusercontent.com/2597375/188733651-94abdb09-96b4-4a81-a121-33045ae86a3c.png)
![Screen Shot 2022-07-29 at 03 05 31__SW36f](https://user-images.githubusercontent.com/2597375/188733701-41658f2d-5178-4e20-9bd8-ea3e18c40fcb.png)


In the example screenshots above, I followed the process that `near login` triggered and logged myself in. Note that, at the end of this process, there should exist a file in the `~/.near-credentials` 


Now, let's check if there's no file existing at the destination:
![Screen Shot 2022-07-29 at 03 06 36__SW37](https://user-images.githubusercontent.com/2597375/188733787-0e7862fa-5ced-4169-b651-1f235658a1d7.png)

If, for any reason, a file already exists at that exact location, then you might wanna back it up somewhere, so that you're not accidentally overwriting something that you don't even know about ;) - but of course the call is yours!

Then let's generate a key pair. You can enter the name of the staking pool you want to use, so it's pre-filled in the output file for you:
![Screen Shot 2022-07-29 at 03 10 09__SW38](https://user-images.githubusercontent.com/2597375/188733838-8ed5c939-21ce-41c9-aa01-5787ca60c8fd.png)

As per the instruction, you'll now need to manually edit the newly created file, and replace `private_key` with `secret_key`
![Screen Shot 2022-09-07 at 03 49 01__SW39](https://user-images.githubusercontent.com/2597375/188735549-6e9de77b-4038-45ad-b11e-00eb7f8540ea.png)

This file should exist in the NEAR workdir on your server machine (next to other config files such as `config.json`)
![Screen Shot 2022-09-07 at 03 45 16__SW40](https://user-images.githubusercontent.com/2597375/188735018-ae5bb20f-2ae6-473f-9ef9-fba4980077b1.png)


Now, if your node has fully synced with the latest blockchain state, we can move on to the last part: making neard an autonomous system service.

------

[Part 5: Making neard an autonomouse system service](https://github.com/near/stakewars-iii/blob/main/challenges/002.md#start-the-validator-node)
===

What we mean by "autonomous system service" is a service that always runs on the background, and will automatically start every time after the server restarts (either on purpose or by accident).
All this is also rather easy if you just follow the instruction as-is. My `/etc/systemd/system/neard.service` file looks like so:
![Screen Shot 2022-09-07 at 03 53 07__SW41](https://user-images.githubusercontent.com/2597375/188736471-a2dcbcd6-5ec3-480f-b53c-9b8708072c72.png)

Once that file is in place, you can go ahead and start `neard` like any standard service (e.g. `systemctl start neard`). You'll also want to make it start automatically on server restart (e.g. `systemctl enable neard`)

Once `neard` is started as a service, you can follow its log like so: `journalctl -n 100 -f -u neard | ccze -A` (the `ccze` part is to support colors in log output, it is optional, but can be helpful at times)
![Screen Shot 2022-09-07 at 03 59 27__SW42](https://user-images.githubusercontent.com/2597375/188738953-bb8c8cfa-4843-46d5-b0b1-6e5f92a55240.png)


------

[Part 6: mount a shardnet staking pool](https://github.com/near/stakewars-iii/blob/main/challenges/003.md#3-mounting-a-staking-pool)
===

Now that the node is running, you need a Staking Pool contract to officially register yourself as a validator node. This was my command & its result:

```
NEAR_ENV=shardnet near call factory.shardnet.near create_staking_pool '{"staking_pool_id": "dklabco", "owner_id": "zur.shardnet.near", "stake_public_key": "ed25519:5Hg5KdmNy5X4K6uCZq686YgPrHk9gVZ6d48JtFXPtmQc", "reward_fee_fraction": {"numerator": 7, "denominator": 100}, "code_hash":"DD428g9eqLL8fWUxv8QSpVFzyHi1Qd16P8ephYCTmMSZ"}' --accountId="zur.shardnet.near" --amount=30 --gas=300000000000000
Scheduling a call: factory.shardnet.near.create_staking_pool({"staking_pool_id": "dklabco", "owner_id": "zur.shardnet.near", "stake_public_key": "ed25519:5Hg5KdmNy5X4K6uCZq686YgPrHk9gVZ6d48JtFXPtmQc", "reward_fee_fraction": {"numerator": 7, "denominator": 100}, "code_hash":"DD428g9eqLL8fWUxv8QSpVFzyHi1Qd16P8ephYCTmMSZ"}) with attached 30 NEAR
Doing account.functionCall()
Retrying request to broadcast_tx_commit as it has timed out [
  'EQAAAHp1ci5zaGFyZG5ldC5uZWFyAKPx0+Cfn9MhH7zBjbaU/ZBvVaMS+8S36Ybn2VIVB2Q8Qy8joQIBAAAVAAAAZmFjdG9yeS5zaGFyZG5ldC5uZWFyXRpZ0zv5rLt/4W4BujQfWuiB9bBEXK6Q+c4uyDn8skcBAAAAAhMAAABjcmVhdGVfc3Rha2luZ19wb29s+QAAAHsic3Rha2luZ19wb29sX2lkIjoiZGtsYWJjbyIsIm93bmVyX2lkIjoienVyLnNoYXJkbmV0Lm5lYXIiLCJzdGFrZV9wdWJsaWNfa2V5IjoiZWQyNTUxOTo1SGc1S2RtTnk1WDRLNnVDWnE2ODZZZ1BySGs5Z1ZaNmQ0OEp0RlhQdG1RYyIsInJld2FyZF9mZWVfZnJhY3Rpb24iOnsibnVtZXJhdG9yIjo3LCJkZW5vbWluYXRvciI6MTAwfSwiY29kZV9oYXNoIjoiREQ0MjhnOWVxTEw4ZldVeHY4UVNwVkZ6eUhpMVFkMTZQOGVwaFlDVG1NU1oifQDAbjHZEAEAAAAA3tgDPEK/0BgAAAAAAACIKI0kWW8sz/KmjnZQOnZ+Du1ko1xolmbHv+TJHLmx5BBwKTMCcxXD0cua6R18yoMU8aSiV+JE5HRGeDdcSUII'
]
Retrying request to broadcast_tx_commit as it has timed out [
  'EQAAAHp1ci5zaGFyZG5ldC5uZWFyAKPx0+Cfn9MhH7zBjbaU/ZBvVaMS+8S36Ybn2VIVB2Q8Qy8joQIBAAAVAAAAZmFjdG9yeS5zaGFyZG5ldC5uZWFyXRpZ0zv5rLt/4W4BujQfWuiB9bBEXK6Q+c4uyDn8skcBAAAAAhMAAABjcmVhdGVfc3Rha2luZ19wb29s+QAAAHsic3Rha2luZ19wb29sX2lkIjoiZGtsYWJjbyIsIm93bmVyX2lkIjoienVyLnNoYXJkbmV0Lm5lYXIiLCJzdGFrZV9wdWJsaWNfa2V5IjoiZWQyNTUxOTo1SGc1S2RtTnk1WDRLNnVDWnE2ODZZZ1BySGs5Z1ZaNmQ0OEp0RlhQdG1RYyIsInJld2FyZF9mZWVfZnJhY3Rpb24iOnsibnVtZXJhdG9yIjo3LCJkZW5vbWluYXRvciI6MTAwfSwiY29kZV9oYXNoIjoiREQ0MjhnOWVxTEw4ZldVeHY4UVNwVkZ6eUhpMVFkMTZQOGVwaFlDVG1NU1oifQDAbjHZEAEAAAAA3tgDPEK/0BgAAAAAAACIKI0kWW8sz/KmjnZQOnZ+Du1ko1xolmbHv+TJHLmx5BBwKTMCcxXD0cua6R18yoMU8aSiV+JE5HRGeDdcSUII'
]
Retrying request to broadcast_tx_commit as it has timed out [
  'EQAAAHp1ci5zaGFyZG5ldC5uZWFyAKPx0+Cfn9MhH7zBjbaU/ZBvVaMS+8S36Ybn2VIVB2Q8Qy8joQIBAAAVAAAAZmFjdG9yeS5zaGFyZG5ldC5uZWFyXRpZ0zv5rLt/4W4BujQfWuiB9bBEXK6Q+c4uyDn8skcBAAAAAhMAAABjcmVhdGVfc3Rha2luZ19wb29s+QAAAHsic3Rha2luZ19wb29sX2lkIjoiZGtsYWJjbyIsIm93bmVyX2lkIjoienVyLnNoYXJkbmV0Lm5lYXIiLCJzdGFrZV9wdWJsaWNfa2V5IjoiZWQyNTUxOTo1SGc1S2RtTnk1WDRLNnVDWnE2ODZZZ1BySGs5Z1ZaNmQ0OEp0RlhQdG1RYyIsInJld2FyZF9mZWVfZnJhY3Rpb24iOnsibnVtZXJhdG9yIjo3LCJkZW5vbWluYXRvciI6MTAwfSwiY29kZV9oYXNoIjoiREQ0MjhnOWVxTEw4ZldVeHY4UVNwVkZ6eUhpMVFkMTZQOGVwaFlDVG1NU1oifQDAbjHZEAEAAAAA3tgDPEK/0BgAAAAAAACIKI0kWW8sz/KmjnZQOnZ+Du1ko1xolmbHv+TJHLmx5BBwKTMCcxXD0cua6R18yoMU8aSiV+JE5HRGeDdcSUII'
]
Retrying request to broadcast_tx_commit as it has timed out [
  'EQAAAHp1ci5zaGFyZG5ldC5uZWFyAKPx0+Cfn9MhH7zBjbaU/ZBvVaMS+8S36Ybn2VIVB2Q8Qy8joQIBAAAVAAAAZmFjdG9yeS5zaGFyZG5ldC5uZWFyXRpZ0zv5rLt/4W4BujQfWuiB9bBEXK6Q+c4uyDn8skcBAAAAAhMAAABjcmVhdGVfc3Rha2luZ19wb29s+QAAAHsic3Rha2luZ19wb29sX2lkIjoiZGtsYWJjbyIsIm93bmVyX2lkIjoienVyLnNoYXJkbmV0Lm5lYXIiLCJzdGFrZV9wdWJsaWNfa2V5IjoiZWQyNTUxOTo1SGc1S2RtTnk1WDRLNnVDWnE2ODZZZ1BySGs5Z1ZaNmQ0OEp0RlhQdG1RYyIsInJld2FyZF9mZWVfZnJhY3Rpb24iOnsibnVtZXJhdG9yIjo3LCJkZW5vbWluYXRvciI6MTAwfSwiY29kZV9oYXNoIjoiREQ0MjhnOWVxTEw4ZldVeHY4UVNwVkZ6eUhpMVFkMTZQOGVwaFlDVG1NU1oifQDAbjHZEAEAAAAA3tgDPEK/0BgAAAAAAACIKI0kWW8sz/KmjnZQOnZ+Du1ko1xolmbHv+TJHLmx5BBwKTMCcxXD0cua6R18yoMU8aSiV+JE5HRGeDdcSUII'
]
Retrying request to broadcast_tx_commit as it has timed out [
  'EQAAAHp1ci5zaGFyZG5ldC5uZWFyAKPx0+Cfn9MhH7zBjbaU/ZBvVaMS+8S36Ybn2VIVB2Q8Qy8joQIBAAAVAAAAZmFjdG9yeS5zaGFyZG5ldC5uZWFyXRpZ0zv5rLt/4W4BujQfWuiB9bBEXK6Q+c4uyDn8skcBAAAAAhMAAABjcmVhdGVfc3Rha2luZ19wb29s+QAAAHsic3Rha2luZ19wb29sX2lkIjoiZGtsYWJjbyIsIm93bmVyX2lkIjoienVyLnNoYXJkbmV0Lm5lYXIiLCJzdGFrZV9wdWJsaWNfa2V5IjoiZWQyNTUxOTo1SGc1S2RtTnk1WDRLNnVDWnE2ODZZZ1BySGs5Z1ZaNmQ0OEp0RlhQdG1RYyIsInJld2FyZF9mZWVfZnJhY3Rpb24iOnsibnVtZXJhdG9yIjo3LCJkZW5vbWluYXRvciI6MTAwfSwiY29kZV9oYXNoIjoiREQ0MjhnOWVxTEw4ZldVeHY4UVNwVkZ6eUhpMVFkMTZQOGVwaFlDVG1NU1oifQDAbjHZEAEAAAAA3tgDPEK/0BgAAAAAAACIKI0kWW8sz/KmjnZQOnZ+Du1ko1xolmbHv+TJHLmx5BBwKTMCcxXD0cua6R18yoMU8aSiV+JE5HRGeDdcSUII'
]
Receipts: 3SmSL8dK9jjfFkDjSNKMAmyYtgLKaZCv5dzeskebBTof, 6iW39NYeZEneV34VoveBrV3LWzLtMsETc1TqJ1tutrRe
	Log [factory.shardnet.near]: The staking pool @dklabco.factory.shardnet.near was successfully created. Whitelisting...
Transaction Id 4SJJjXk7SKpa37FtNE7keYePEVr6EdjXpUDVGHjg3TSS
To see the transaction in the transaction explorer, please open this url in your browser
https://explorer.shardnet.near.org/transactions/4SJJjXk7SKpa37FtNE7keYePEVr6EdjXpUDVGHjg3TSS
```

Once the Staking Pool contract was created, anyone can stake money to your pool (including yourself aka self-stake). The command & the result for me was like so:

```
NEAR_ENV=shardnet near call dklabco.factory.shardnet.near deposit_and_stake --amount 502 --accountId zur.shardnet.near --gas=300000000000000
Scheduling a call: dklabco.factory.shardnet.near.deposit_and_stake() with attached 502 NEAR
Doing account.functionCall()
Retrying request to broadcast_tx_commit as it has timed out [
  'EQAAAHp1ci5zaGFyZG5ldC5uZWFyAJK6q9Nv36VvBvjnUgQ0zS1RHF6Nn1jpkyO78HPJi3j9gV0qousAAAAdAAAAZGtsYWJjby5mYWN0b3J5LnNoYXJkbmV0Lm5lYXJKU+0RONpS+G8xWWLCjEDxBMtfKAmFk2khQ48cidYqcQEAAAACEQAAAGRlcG9zaXRfYW5kX3N0YWtlAgAAAHt9AMBuMdkQAQAAAAC2+dmFh6I+nwEAAAAAAL7P5+dxJT+ts9dFAL6jOHS3yxkiF8Di+OC1wJRxdCB0zY99sywqBQitdcezw5GxRa1XZapQ/2hLKZygUmVtggE='
]
Receipts: 4tjeD9VE7bU5Rbk9cCtRMfAyujTwv8qKnH8nDFfP73Jx, 5TwKKhvcAoZ2ZV4hHC1qARKW2obw2tmvX1Z1Tgiu7TPM, CD8Y1AvEnTtjvaWbhFw6QR3xbbtmoRuaWLCeEofbLux5
	Log [dklabco.factory.shardnet.near]: @zur.shardnet.near deposited 502000000000000000000000000. New unstaked balance is 502000000000000000000000000
	Log [dklabco.factory.shardnet.near]: @zur.shardnet.near staking 502000000000000000000000000. Received 502000000000000000000000000 new staking shares. Total 0 unstaked balance and 502000000000000000000000000 staking shares
	Log [dklabco.factory.shardnet.near]: Contract total staked balance is 531999999999999000000000000. Total number of shares 531999999999999000000000000
Transaction Id AGAjfmc3fFRAZ5dZeWGuYiNU4YtfS49XCsGwxiYfPNmR
To see the transaction in the transaction explorer, please open this url in your browser
https://explorer.shardnet.near.org/transactions/AGAjfmc3fFRAZ5dZeWGuYiNU4YtfS49XCsGwxiYfPNmR
''
```

The above suggests that I've successfully self-staked (self-delegated) my first 502 NEAR tokens to my shardnet validator node.

Please feel free to [explore](https://github.com/near/stakewars-iii/blob/main/challenges/003.md#unstake-near) [the](https://github.com/near/stakewars-iii/blob/main/challenges/003.md#withdraw) [remaining](https://github.com/near/stakewars-iii/blob/main/challenges/003.md#ping) [contract](https://github.com/near/stakewars-iii/blob/main/challenges/003.md#staked-balance) [calls](https://github.com/near/stakewars-iii/blob/main/challenges/003.md#unstaked-balance) [as](https://github.com/near/stakewars-iii/blob/main/challenges/003.md#available-for-withdrawal) [mentioned](https://github.com/near/stakewars-iii/blob/main/challenges/003.md#pause--resume-staking) in the stakewars challenge guideline.

An example of calling the method `is_account_unstaked_balance_available` on my staking pool for one of my delegator accounts:
![Screen Shot 2022-09-07 at 05 03 10__SW43](https://user-images.githubusercontent.com/2597375/188748548-919cc90d-19fd-41fc-bafc-9df893a2c3b1.png)

---

[Part 7: reading status from your validator node](https://github.com/near/stakewars-iii/blob/main/challenges/004.md)
===

There are various things to care about when it comes to reading validator node status. The RPC API is one widely used approach. Here's an example of reading the `neard` build version:

![Screen Shot 2022-09-07 at 04 43 05__SW44](https://user-images.githubusercontent.com/2597375/188748866-7de9232c-d676-48cb-812f-6dad1d467930.png)

Note that the utilities `curl` and `jq` are used in these example operations, so you might want to install them first with `apt install curl jq`.

Using the `near-cli` itself is also a popular way to extract very useful data out of the node.

Here's how to check on the delegators to your validator node:

![Screen Shot 2022-09-07 at 04 44 00__SW45](https://user-images.githubusercontent.com/2597375/188749200-385dcbe5-5560-4337-95f6-d6ce2726e49f.png)

And here's an example call to check why your node will be kicked out of the active validator pool:

![Screen Shot 2022-09-07 at 05 17 49__SW46](https://user-images.githubusercontent.com/2597375/188750360-5337d55e-a07d-4898-8a59-923dc54b832c.png)

Note that the call above returned "empty" because the node will not be kicked out due to any malperformance during the current epoch.
If the node performs poorly, the "kickout reason" shall be displayed like in this example:

```
curl -s -d '{"jsonrpc": "2.0", "method": "validators", "id": "dontcare", "params": [null]}' -H 'Content-Type: application/json' 176.9.84.61:3030 | jq -c '.result.prev_epoch_kickout[] | select(.account_id | contains ("dklabco.factory.shardnet.near"))' | jq .reason
{
  "NotEnoughChunks": {
    "expected": 3,
    "produced": 1
  }
}
```

------



