# Wallet Memonic
# When you download the app, this is generated for you automatically or you can import an exisiting one
MEMONIC="arm riot arch tag salute alarm bless nephew maple inner glue spy nephew offer vehicle hospital destroy tackle seed sell speak extend steel above feed"

# Create Accounts x3
goal account new -d data/relay

# Account 1 - Patient
ALGOHEALTH_PATIENT=PU4TY72ONIP2XNGHYHAH34PED4NIBFESNI2VGZBPJVDFN2O6VCI56LOOYM
# Account 2 - Doctor
ALGOHEALTH_DOCTOR=HXM7DMME5MMFPT5JPWH2GHMPZ7TNPVZP4NBAFEZDNIYYPOHAOAX3HDDVRM
# Account 3 - Supplier
ALGOHEALTH_SUPPLIER=6ZZCEAVTWMXDDLZXUO5U2OIBCAT46XAMO6IH7ZRAYRPZIGR7OYNDLPJ7XU

# Get Status - Successfully connected to Algorand
goal node status -d data/relay/
goal account list -d data/relay/

# Create 3 Way MultiSig account between Patient, Doctor and Supplier
goal account multisig new $ALGOHEALTH_PATIENT $ALGOHEALTH_DOCTOR $ALGOHEALTH_SUPPLIER -k 3               -d data/relay/

# New join account from Multi Sig
ALGOHEALTH_MULTISIG=M7XM433LZCZIJTE3GPKORTUVWOMQUXPHAOBX3H4A5CD3XXZKFB4HKEQV5A

# Doctors script / message to be included in transaction (Can be encrypted using patients public key)
TX_MSG="Send_20_HealthAid"

# Patient sends money to MultiSig account for medicine (Can also be funded by donations and insurance)
goal clerk send -a 300000 -f $ALGOHEALTH_PATIENT -t $ALGOHEALTH_MULTISIG -d data/relay/

# We generate a 3 way transaction for delivery of medicine
goal clerk send -a 30000 -f $ALGOHEALTH_MULTISIG -t $ALGOHEALTH_SUPPLIER -o ./RawMultiSig.tx -d data/relay/ -n $TX_MSG

# 1) First the doctor signs the initial script. Transaction file is sent via end to end encryption to the supplier.
goal clerk multisig sign -t ./RawMultiSig.tx -a $ALGOHEALTH_DOCTOR -d data/relay/

# 2) Supplier signs on filling of script and funding of join account.
goal clerk multisig sign -t ./RawMultiSig.tx -a $ALGOHEALTH_SUPPLIER -d data/relay/

# 3) On delivery or pickup, final signiture is used to authenticate doctor to patient delivery.
goal clerk multisig sign -t ./RawMultiSig.tx -a $ALGOHEALTH_PATIENT -d data/relay/

# Send msg to algorand and unlock multisig
goal clerk rawsend -f ./RawMultiSig.tx -d data/relay/

# Final transaction on testnet
# https://algoexplorer.io/tx/
