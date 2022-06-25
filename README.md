# Smart Contract Developer  - ITU Blockchain Devs

In this lesson, we examined the ERC-721 token standard after ERC-20. After comparing it with ERC-20, we reviewed the standard contract prepared by OpenZeppelin. Then we wrote a simple ERC721 token contract.

<br/>

# ERC-721 token documentations

* [OpenZeppelin ERC-721 contract](https://github.com/OpenZeppelin/openzeppelin-contracts/tree/master/contracts/token/ERC721)
* [OpenZeppelin Token Documentations](https://docs.openzeppelin.com/contracts/4.x/tokens)
* [Ethereum Documentations](https://ethereum.org/en/developers/docs/standards/tokens/)





### Token URI
Returns the unique URI of each token
``` javascript
    function tokenURI(uint256 tokenId) public view virtual override returns (string memory) {
        _requireMinted(tokenId);
        string memory baseURI = _baseURI();
        return bytes(baseURI).length > 0 ? string(abi.encodePacked(baseURI, tokenId.toString())) : "";
    }
```


### Balance Of
It is used to query the balance of token holders.
``` javascript
    function balanceOf(address account) public view virtual override returns (uint256) {
        return _balances[account];
    }
```

### Owner Of
Returns the address of the token holder.

``` javascript
    function ownerOf(uint256 tokenId) public view virtual override returns (address) {
        address owner = _owners[tokenId];
        require(owner != address(0), "ERC721: invalid token ID");
        return owner;
    }
```


### Approve
It gives the right to other users to spend the user's tokens. This 'confirmed' amount is stored in _tokenApprovals.

``` javascript
    function approve(address to, uint256 tokenId) public virtual override {
        address owner = ERC721.ownerOf(tokenId);
        require(to != owner, "ERC721: approval to current owner");

        require(
            _msgSender() == owner || isApprovedForAll(owner, _msgSender()),
            "ERC721: approve caller is not token owner nor approved for all"
        );

        _approve(to, tokenId);
    }
```

### Get Approved
With this function, the approved token can be queried.

``` javascript
    function getApproved(uint256 tokenId) public view virtual override returns (address) {
        _requireMinted(tokenId);

        return _tokenApprovals[tokenId];
    }
```

### Set Approval For All
It gives the right to other users to spend the user's tokens. The difference from Approve is that it provides rights not only to one token, but also to all tokens directly. This 'approved' amount is stored in _operatorApprovals.

``` javascript
    function setApprovalForAll(address operator, bool approved) public virtual override {
        _setApprovalForAll(_msgSender(), operator, approved);
    }
```

### Is Approved For All
Bu fonksiyon ile kullanımına onay verilmiş token sorgulanabilir.
With this function, the approved token can be queried.

``` javascript
    function isApprovedForAll(address owner, address operator) public view virtual override returns (bool) {
        return _operatorApprovals[owner][operator];
    }
```

### Transfer From
Approve edilmiş haklar üzerinden transfer edilebilmesini sağlayan fonksiyon. Esasen diğer kullanıcıların sizin onayladığınız tokenleri nasıl harcayacağı sorusunun cevabıdır.
Function that allows transfer over the granted rights. It is essentially the answer to the question of how other users will spend the tokens that you approve.

``` javascript
    function transferFrom(address from,address to, uint256 tokenId) public virtual override {
        require(_isApprovedOrOwner(_msgSender(), tokenId), "ERC721: caller is not token owner nor approved");
        _transfer(from, to, tokenId);
    }
```


### Safe Transfer From
Bu fonksiyonda aynı transferFrom gibi çalışır. Ondan farkı ise tokenın gönderildiği kontratın ERC721Receiver'a uygun olup olmadığının kontrolünü yapar.

In this function it works just like transferFrom. The difference from it is that it checks whether the contract to which the token is sent is suitable for the ERC721Receiver.

``` javascript
    function safeTransferFrom(address from, address to, uint256 tokenId) public virtual override {
        safeTransferFrom(from, to, tokenId, "");
    }
```
<br/>

[Video ](https://www.youtube.com/watch?v=zf4orRramo4)

[Project Path](./ERC-721)
