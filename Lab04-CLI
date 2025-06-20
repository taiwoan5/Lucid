================Lab 04- Tạo tokens/NFT =============
#Step 1: =========Gán các biến==================
apt update
apt install xxd


testnet="--testnet-magic 2"
address=$(cat base.addr)
cardano-cli query utxo $testnet --address $address


txhash=3d43bf1d3d892650faa0206ab1428c6dd040f52bd6d9e0a51649664a2a047a14
txix=1
output=100000000


ipfs_hash="QmTPoBBis8n6EmkQv554DHZMjgECycb6mrTsAMBbFrnSNX"
realtokenname="NGUYENVANTAI_13"
tokenname=$(echo -n $realtokenname| xxd -ps | tr -d '\n')
tokenamount=1


#Step 1: =========Tạo Policy ID===============
mkdir tokens; cd tokens
mkdir policy

cardano-cli address key-gen \
    --verification-key-file policy/policy.vkey \
    --signing-key-file policy/policy.skey

	
touch policy/policy.script && echo "" > policy/policy.script
echo "{" >> policy/policy.script
echo "  \"keyHash\": \"$(cardano-cli address key-hash --payment-verification-key-file policy/policy.vkey)\"," >> policy/policy.script
echo "  \"type\": \"sig\"" >> policy/policy.script
echo "}" >> policy/policy.script



##====Đọc lại nội dung file policy.script để kiểm tra==============
cat policy/policy.script

cardano-cli conway transaction policyid --script-file ./policy/policy.script > policy/policyID/var/folders/wq/v4n7xjh53njcdrlzxw7454200000gn/T/TemporaryItems/NSIRD_screencaptureui_jeGN0Q/Ảnh màn hình 2025-03-22 lúc 11.28.01.png
cat policy/policyID
policyid=$(cat policy/policyID)









#Step 2: =========Tạo Metadata ===============

echo "{" >> metadata.json
echo "  \"721\": {" >> metadata.json
echo "    \"$(cat policy/policyID)\": {" >> metadata.json
echo "      \"$(echo $realtokenname)\": {" >> metadata.json
echo "        \"description\": \"C2VN_BK02 \"," >> metadata.json
echo "        \"name\": \"Vu Van Nam\"," >> metadata.json
echo "        \"id\": \"31\"," >> metadata.json
echo "        \"image\": \"ipfs://$(echo $ipfs_hash)\"" >> metadata.json
echo "      }" >> metadata.json
echo "    }" >> metadata.json
echo "  }" >> metadata.json
echo "}" >> metadata.json

cat metadata.json 

#Step 3: =========Soạn thảo giao dịch===============

echo $policyid.$tokenname >policy_token.log

cardano-cli conway transaction build $testnet \
--tx-in $txhash#$txix \
--tx-out $address+$output+"$tokenamount $policyid.$tokenname" \
--change-address $address \
--mint "$tokenamount $policyid.$tokenname" \
--mint-script-file policy/policy.script \
--metadata-json-file metadata.json  \
--witness-override 2 \
--out-file mint-nft.raw

#Step 4: =========Tạo ký giao dịchdịch===============
cardano-cli conway transaction sign  $testnet \
--signing-key-file ../payment.skey  \
--signing-key-file policy/policy.skey  \
--tx-body-file mint-nft.raw \
--out-file mint-nft.signed

#Step 5: =========Gửi giao dịchdịch===============

cardano-cli conway transaction submit $testnet --tx-file mint-nft.signed



cardano-cli query utxo --address $(< payment.addr)
                           TxHash                                 TxIx        Amount
--------------------------------------------------------------------------------------
d4b158e58cb58da28b25837300f6ef8f9f7d67fd5a5ce07648d17a6fae31b88a     0        10000000 lovelace + 1000 11375f8ee31c280e1f2ec6fe11a73bca79d7a6a64f18e1e6980f0c74.637573746f6d636f696e
d4b158e58cb58da28b25837300f6ef8f9f7d67fd5a5ce07648d17a6fae31b88a     1        9989824379 lovelace + TxOutDatumNone

cardano-cli conway transaction build \
--tx-in d4b158e58cb58da28b25837300f6ef8f9f7d67fd5a5ce07648d17a6fae31b88a#0 \
--tx-in d4b158e58cb58da28b25837300f6ef8f9f7d67fd5a5ce07648d17a6fae31b88a#1 \
--tx-out addr_test1vp9khgeajxw8snjjvaaule727hpytrvpsnq8z7h9t3zeuegh55grh+1043020+"1 11375f8ee31c280e1f2ec6fe11a73bca79d7a6a64f18e1e6980f0c74.637573746f6d636f696e" \
--tx-out $(< payment.addr)+8956980+"999 11375f8ee31c280e1f2ec6fe11a73bca79d7a6a64f18e1e6980f0c74.637573746f6d636f696e" \
--change-address $(< payment.addr) \
--out-file tx.raw


cardano-cli conway transaction sign --tx-file tx.raw --signing-key-file policy.skey --signing-key-file payment.skey --out-file tx.signed


cardano-cli conway transaction submit --tx-file tx.signedTransaction successfully submitted.
