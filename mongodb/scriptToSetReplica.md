Ubuntu-based VPS-এ MongoDB-র replica set সেটআপ অটোমেটেডভাবে করার script । এই স্ক্রিপ্ট:

- mongod.conf ফাইল আপডেট করে

- MongoDB restart করে

- replica set initiate করে দেয়

📜 স্ক্রিপ্ট: setup-replica.sh
```bash
#!/bin/bash

# ====== CONFIGURATION ======
REPLICA_NAME="rs0"
MONGOD_CONF="/etc/mongod.conf"
SERVER_IP=$(hostname -I | awk '{print $1}')
# ===========================

echo "🔧 Updating mongod.conf to enable replica set..."

# Backup existing config
sudo cp $MONGOD_CONF ${MONGOD_CONF}.backup

# Add replication config if not present
if ! grep -q "replication" $MONGOD_CONF; then
  echo -e "\nreplication:\n  replSetName: \"$REPLICA_NAME\"" | sudo tee -a $MONGOD_CONF
else
  sudo sed -i "s/replSetName:.*/replSetName: \"$REPLICA_NAME\"/" $MONGOD_CONF
fi

# Add bindIp if necessary
if ! grep -q "$SERVER_IP" $MONGOD_CONF; then
  sudo sed -i "/bindIp:/ s/$/,${SERVER_IP}/" $MONGOD_CONF
fi

echo "✅ Config updated. Restarting MongoDB..."

# Restart MongoDB
sudo systemctl restart mongod

sleep 3

echo "🚀 Initiating Replica Set..."

mongosh --eval "rs.initiate({_id: '$REPLICA_NAME', members: [{ _id: 0, host: '$SERVER_IP:27017' }]})"

echo "✅ Replica Set initiated. You can check with: rs.status()"
```
📦 স্ক্রিপ্ট রান করার নিয়ম
স্ক্রিপ্টটি setup-replica.sh নামে VPS-এ সেভ করো:

```bash
nano setup-replica.sh
```
→ এরপর উপরের স্ক্রিপ্টটা কপি-পেস্ট করো।

স্ক্রিপ্ট executable করো:

```bash
chmod +x setup-replica.sh
```
স্ক্রিপ্ট রান করো:

```bash 
sudo ./setup-replica.sh
```
✅ পরবর্তী ধাপ
স্ক্রিপ্ট রান হওয়ার পর mongosh এ ঢুকে নিচের কমান্ড দিয়ে নিশ্চিত হও:

```js
rs.status()
```
🧠 Bonus: Node.js থেকে কনেকশনের সময় কীভাবে URI লিখবে
```js
// const uri = "mongodb://<username>:<password>@your-vps-ip:27017/your-db-name?replicaSet=rs0";
const uri = "mongodb://localhost:27017/your-db-name?replicaSet=rs0"

```
