![Linux File System Hierarchy](https://github.com/khasan2k/DevOps/blob/6cf60ce5fc04049304511cfeaf6d98d180411eda/Images/download%20(3).png)

Linux File System Hierarchy: সহজ করে বুঝি

Linux এ সবকিছু একটা গুছানো ফোল্ডার স্ট্রাকচারে রাখা হয়, যেটাকে বলে Filesystem Hierarchy Standard (FHS)। এই স্ট্রাকচারটা এমনভাবে বানানো, যেন ইউজার আর সফটওয়্যার—দুজনেই জানে কোন ফাইল কোথায় থাকবে। নিচে প্রতিটা ডিরেক্টরির কাজটা সহজ করে ব্যাখ্যা করছিঃ 

🔸 / – এইটা হলো Root Directory
সবকিছুর শুরুর জায়গা। Windows এ যেমন C drive থাকে, Linux এ ঠিক তেমনই/ মানে পুরো সিস্টেমের মূল জায়গা।

🔸 /bin – Binary Executables
এখানে পাওয়া যায় সিস্টেম চালানোর জন্য দরকারি কমান্ডগুলো যেমন:ls, cp, mv ইত্যাদি।

🔸 /boot – Boot Files
Linux চালু হওয়ার সময় যেসব ফাইল দরকার, যেমন kernel, grub config—সেগুলো এখানে থাকে। 

🔸 /dev – Device Files
হার্ডডিস্ক, পেনড্রাইভ বা অন্য যে কোনো ডিভাইস /dev এর ভেতরে ফাইল হিসেবে থাকে। যেমন /dev/sda হচ্ছে একটা হার্ডডিস্ক।

🔸 /etc – Configuration Files
System-wide সব সেটিংস ফাইল এখানে থাকে। যেমন /etc/passwd দিয়ে ইউজার ইনফো রাখা হয়, /etc/hostname দিয়ে ডিভাইসের নাম।

🔸 /home – User Files
সব ইউজারের নিজস্ব আলাদা ফোল্ডার এখানে থাকে। যেমন /home/anik আমার ইউজার ফোল্ডার হতে পারে।

🔸 /lib – Libraries
/bin আর /sbin এর প্রোগ্রামগুলোর দরকারি লাইব্রেরিগুলো এখানে থাকে। Windows এ যেমন .dll ফাইল থাকে, তেমন।

🔸 /media – Removable Media
Pendrive, CD, DVD ইত্যাদি connect করলে অটো এই ডিরেক্টরির নিচে mount হয়।

🔸 /mnt – Manual Mount
নিজে থেকে কোন ফাইলসিস্টেম মাউন্ট করলে সাধারণত এখানে করা হয়।

🔸 /opt – Optional Software
Third-party বড়সড় সফটওয়্যার বা আলাদা প্যাকেজ গুলো এখানে রাখা হয়।

🔸 /proc – Process Info
এখানে কোনো রিয়েল ফাইল থাকে না, সব ভার্চুয়াল। System আর running process সম্পর্কিত তথ্য এখানে পাওয়া য/proc/cpuinfo দিয়ে CPU info।

🔸 /root – Root User's Home
এটা root (admin) ইউজারের হোম ডিরেক্টরি।/home এর বাইরে রাখার কারণ, সিস্টেমে সমস্যা হলেও root ইউজার যেন এক্সেস পায়।

🔸 /run – Runtime Data
সিস্টেম চালু হওয়ার পর যেসব ডাটা টেম্পরারি লাগে (যেমন PID) সেগুলো এখানে থাকে।

🔸 /sbin – System Binaries
Admin দের দরকারি কমান্ড এখানে থাকে, যেমifconfig, fsck। এগুলো সাধারন ইউজার ইউজ করতে পারে না।

🔸 /srv – Service Data
যেমন web server চালালে এর ওয়েব ফাইল গুলো /srv/www এর মধ্যে থাকে।

🔸 /sys – System Hardware Info
এই ডিরেক্টরি দিয়ে হার্ডওয়্যার সম্পর্কিত ডাটা পাওয়া যায়।/proc এর মতোই virtual, তবে hardware focused।

🔸 /tmp – Temporary Files
প্রোগ্রাম বা সিস্টেম যখন কোনো অস্থায়ী ফাইল বানায়, তখন সেটা এখানে রাখে। রিবুট দিলে মুছে যায়।

🔸 /usr – User Software & Resources
এটা অনেক বড় একটা ডিরেক্টরি। এখানে থাকে—/usr/bin: Normal ইউজারের ইউজ করার মতো প্রোগ্রাম

/usr/sbin: System admin ইউজারদের টুল
/usr/lib: Program library
/usr/share: Manual, icon, locale info ইত্যাদি

এভাবে সাজানো থাকার কারণে Linux এ আপনি খুব সহজেই বুঝতে পারবেন কোন ফাইল কোথায় এবং কেন আছে।
