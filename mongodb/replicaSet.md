MongoDB-তে transaction চালাতে হলে অবশ্যই replica set enable করতে হয়। VPS-এ MongoDB-র replica set চালু করার জন্য নিচের ধাপগুলো অনুসরণ করো। আমি ধাপে ধাপে বাংলায় বুঝিয়ে দিচ্ছি:

🔧 ১. MongoDB কনফিগারেশন ফাইল আপডেট করো
প্রথমে VPS-এ লগইন করো এবং MongoDB-র কনফিগারেশন ফাইলটি খুলো:

```
sudo nano /etc/mongod.conf
```
এর মধ্যে নিচের মতো replication অংশটি যোগ করো (なথাকলে নতুন করে লিখো):

```
replication:
  replSetName: "rs0"
```

🔄 ২. MongoDB সার্ভার রিস্টার্ট করো
```
sudo systemctl restart mongod
```
🧠 ৩. Replica Set Initiate করো
এখন MongoDB shell এ ঢুকে replica set শুরু করো:

```
mongosh
```
তারপর নিচের কমান্ডটি দাও:

```
rs.initiate()
```
এতে একটা ডিফল্ট replica set rs0 তৈরি হবে (আমরা যেটা কনফিগারেশনে দিয়েছি)।

✅ ৪. যাচাই করো replica set চলছে কিনা
```
rs.status()
```
এতে ok: 1 দেখালে বুঝবে replica set কাজ করছে।

🧪 এখন তুমি transaction ইউজ করতে পারবে
MongoDB এখন multi-document transaction সাপোর্ট করবে, যেহেতু replica set enable হয়েছে।

✨ অতিরিক্ত টিপস
Production এ multiple nodes setup করা উচিত, কিন্তু ডেভেলপমেন্ট বা সাধারণ ব্যবহারে single-node replica set কাজ করে।

যদি Node.js (Mongoose) বা অন্য লাইব্রেরি ইউজ করো, connection URI তে replicaSet=rs0 যুক্ত করো।

উদাহরণ:

```
mongodb://localhost:27017/yourDB?replicaSet=rs0
```
