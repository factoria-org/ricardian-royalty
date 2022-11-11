# Ricardian NFT Royalty

> Legally Binding EIP-2981 NFT Royalty Standard

Ricardian NFT Royalty Standard is an extension to the existing [EIP-2981 NFT Royalty Standard](https://eips.ethereum.org/EIPS/eip-2981) that's aimed at providing a legal framework for NFT creators, in order to enforce their intellectual property rights properly, even when the royalty standard is not enforceable on-chain.

```javascript
// EIP-2981 standard
function royaltyInfo(
  uint tokenId,
  uint value
) external view returns (address receiver, uint256 royaltyAmount) {
  return(owner(), value * 3 / 100);  // 3% royalty
}


// Ricardian Extension
// Links to a legal document explaining the property rights for the tokenId
function licenseURI(uint tokenId) external view returns (string memory uri) {
  return "ipfs://..";
}
```

---

# Problem

The [EIP-2981 NFT Royalty Standard](https://eips.ethereum.org/EIPS/eip-2981) is **"just a convention"**, and there is no way to enforce it.

As a result, several NFT marketplaces have started bypassing the standard.

We can make a case that this is not much different from stealing intellectual properties, similar to:

1. Musicians sampling other musician's songs and not giving them royalty.
2. Toy companies making money selling Disney character toys, WITHOUT paying Disney any royalty.

This would be a different story if the smart contract itself DID NOT have any royalty declaration. But all NFT contracts with EIP-2981 support DO declare that ALL their NFTs must follow the specified royalty scheme.

So why does this problem even exist? Wouldn't all this be illegal and the marketplaces would be somehow disincentivized from ignoring these property rights?

---

# Why does the problem exist?

**As it stands, disrespecting EIP-2981 is technically NOT illegal**, because the EIP-2981 standard does not include any license document and is merely a "convention".

Just like how an open source project without a license document is regarded as public domain, it is no surprise that a digital item (NFT) without a license document is regarded as "public domain" and anyone can treat them whichever way they want.

Because it's technically not illegal, there is nothing stopping all the NFT marketplaces from disrespecting the intellectual property rights, especially if it means they can gain more market share.

And this spreads virally throughout the industry because all NFT marketplaces must think about the following problem:

1. Other NFT marketplaces are doing it, and if they don't join, they will lose market share. Might as well join it instead of coming out as a loser in the wild wild west.
2. Even the NFT marketplaces that WANT to support NFT creators' rights have to come up with a far-from-optimal solution because **they can no longer rely on benevolence of the others.** They must assume that intellectual properties without a proper license will not be enforced.

---

# Solution

**Imagine EIP-2981 NFT royalty standard, but legally enforceable.**

This proposal simply adds a single additional interface to the existing EIP-2981 to turn ANY NFT contract into a [Ricardian contract](https://en.wikipedia.org/wiki/Ricardian_contract).

Basically, in addition to the existing `royaltyInfo()` method:

```javascript
function royaltyInfo(
  uint tokenId,
  uint value
) external view returns (address receiver, uint256 royaltyAmount) {
  return(owner(), value * 3 / 100);  // 3% royalty
}
```

We introduce one additional method called `licenseURI()`:

```javascript
function licenseURI(uint tokenId) external view returns (string memory uri) {
  return "ipfs://..";
}
```

With this in place, the NFT creators can now legally make claims against NFT marketplaces who disrespect their royalty declaration, as well as the purchasers who buy the NFTs.

Note that you do NOT have to implement this method. This is just an opt-in based solution. If you don't implement it, it simply means the NFT has no specific license (and you are OK with anyone treating it as a public domain digital object). If you do implement it, you are explicitly stating the terms under which the items are traded on the market.

The `ipfs://..` URL should contain a license document that's legally binding. Something along the lines of: "By owning this token, you agree that you will pay the royalty amount specified by the `royaltyInfo()` contract method for this tokenId."

---

# Implementation details

For example, any marketplace that wants to implement this standard may simply check the `licenseURI()` view method when deciding whether to list them on their marketplace automatically.

The important point here is that this decision is made by each individual marketplace, not by the NFT creators.

1. This is much more scalable than each individual NFT creator implementing some special logic
2. This is more decentralized, as it's up to each marketplace to decide whether to respect the license or not, and each marketplace can be held accountable for their decisions.


---

# Final thoughts

Of course, this is by no means the perfect solution. There will still be marketplaces that ignore these standards. But at least this approach provides a useful tool to the creators and marketplaces:

1. **Giving creators a legal framework to protect their own rights:** Give each creator legally binding rights to protect their own intellectual property. 
2. **Prisoner's dilemma:** NFT marketplaces don't have to worry that respecting creators' property rights will only make themselves lose market share with nothing to gain. Because they now know that each NFT creator can do something about their property rights ON THEIR OWN (such as legally requesting disrespecting marketplaces to delist items)
3. **Creator oriented. Not marketplace oriented:** This solution is not centered around specific marketplaces. Just like the EIP-2981 standard, you declare the rights **on the NFT contract side**, and each marketplace decide whether to respect them or not.

Whether a marketplace decides to respect this or not depends on the nature of the marketplace, but at least the creators now can do something about it, without relying on marketplaces.


---
