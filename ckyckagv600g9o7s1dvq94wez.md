# Let’s Talk About Backups

I am a certified Backup and Disaster Recovery Expert and have worked in Backup and Disaster Recovery for many years using various software and strategies. 

Backups are the most critical part of any business. I have helped dozens of businesses create backup and disaster recovery strategies, and have recovered businesses using these backups many times. Those businesses that had a valid and verified backup in place were able to achieve 100% successful recovery with little to no data loss. The businesses who called me and didn't have a backup, were in a far worse situation and business survival was questionable. 

This article is to educate you about Backups in general and is not a recommendation for any particular strategy or software solution. 

### What is a backup? 
A backup is a copy of your important data that is kept on separate media from the original source. A simple example, you copy your current project to a USB thumb drive and keep that USB drive in your safe at home. This copy of the data is kept separate from the original device and therefore is a valid backup that could be used to restore the project as of the last time the backup was made. 

There are many ways to accomplish this and it will be explored in further detail below so for now, just focus on knowing the fundamental concept that it's a copy of the data stored somewhere else. 

### Why backup?
You may ask yourself, why should I back up my data?  Maybe you just bought a brand new computer so you don't feel like it will fail anytime soon and you decide not to backup. Maybe you use a RAID storage system with multiple redundant drives so you feel that your data is safe.  Even on a brand new device or a RAID storage system, you need to keep a backup copy of your important data. 

Possible failures requiring restoring from backup:
- Hardware Failures
- Natural Disasters (Storms, Floods, Hurricanes, Volcano, Avalanche, Earthquake)
- Human Error
- Viruses 
- Cybersecurity issues such as Ransomware 


### Types of backups 
There are several different types of backups and the one you use will be determined base on your specific business needs. I have recommended each of these in different scenarios based on stakeholder requirements and business objectives. 

**Manual**
A manual backup is one consciously performed by an assigned individual. These can be one-time, or scheduled. The key part here is that a human must initiate the backup or it doesn't happen.  This is typically recommended for data that doesn't change often, and is manually backed up when it does change. 

**Scheduled**
A scheduled backup is a recurring backup task that runs on a set interval. Some businesses run this daily, weekly, monthly, quarterly or annually. This is a safety net backup to capture changes made on a interval basis. This is recommended when infrequent changes are made to data and you want to keep an up to date backup without a human having to perform the backup. 

**Incremental**
An incremental backup is a chain of backups that are strung together to form the basis of a backup with multiple recovery points. The first backup is a _full_ backup where as the entire system is backed up. Incremental backups are then set at a specific interval, some as often as every 15 minutes for rapidly changing data to every 60 minutes or even every 24 hours. Each incremental backup only backs up the data that changed between the last incremental backup and the current one.  This type of backup is ideal for data that is constantly being updated and should be set to record those changes as often as possible. 

**Differential**
A differential backup is a series of backups where a previous backup is required. The differential backup will look at the last backup, and the current data, and create a backup of only what has changed. This is different from an incremental as these are generally ran in larger time intervals such as weekly or monthly. 

### Backup Media
There are multiple ways of storing backups, the critical point here is that it must be stored some where other than the source of your data. If you are backing up your computer, then the backup cannot reside on your computer. You should consider the risk of your backup device failing when planning your backup.

![An assortment of computer storage devices including multiple thumb drives, SD Card, Floppy Disk, CD, and hard disk drive ](https://cdn.hashnode.com/res/hashnode/image/upload/v1642050286656/NtnIqX2GB.jpeg)

Choosing the right backup media involves considering the amount of data that you will be backing up, the frequency of updating that backup, the length of time you will be storing that data, and the cost of storing the data. 

Rate of Change (RoC)
You also need to consider your backup strategy and account for the Rate of Change.  The RoC applies to **incremental** and **differential** backups and is the amount of data that is changed each month on the device you are backing up. Manual backups typically overwrite previous backups and therefore the RoC is not as relevant but the storage media needs to have more space than is needed for the backup. 

Example, if the initial backup is 150GB and the rate of change is 15GB monthly, then you need to plan for an extra 180GB per year of storage, and to be safe I would, at the very least, double that to 360GB plus the initial 180GB. 

- Tape Backups
These are not used as much in today's day and age, but some businesses are still using them. 
- Disk Backups
This is typically a hard drive that is connected to the computer via USB or other means. I recommend that RAID be used for disk backup to ensure redundancy of the backup media. 
- USB Drive / Thumb Drive
USB Drives can hold a surprising amount of data and can be a good backup for some users. 
- NAS / Network Backup
Some businesses may have Network Attached Storage (NAS) or another device on the network with storage that can be used to store your backups and is connected via local network. 
- FTP 
This is a connection over the internet to another device for storing data. 
- Cloud Backup
There are a huge number of cloud backup services that allow you to store your data in another companies cloud. Typically you pay a monthly rate for a set amount of data or a rate per unit of storage (ex. $0.25/GB).

### Backup Storage ###
However you decide to store your backup, one important thing to keep in mind is that the best strategy involves storing the backup somewhere other than the business itself. If you are not using a cloud backup service and are using a physical storage such as an external drive then this should be kept off-site to ensure that during a disaster, the backup is available for use. 

My suggestion is that you keep a copy of the data in a geographically separate data center that is at least 90-miles from the business itself. This would ensure that during most natural disasters, the backup itself is valid even if the building was destroyed. For example, if you are in the West Coast of USA then you would want to store you data in the Mid-West or East Coast. 

Alternatively you can also setup geo-redundant backup copies where your data is stored in more than 1 data-center across the country and/or in another country all together. Again, the answer to this question should be decided by the stakeholders of the business to determine how important their data is during a recovery. 

### Backup Security
![Three keys painted to look like computer circuit boards, one is black and white, the next is green orange and white, the third is red blue and white. ](https://cdn.hashnode.com/res/hashnode/image/upload/v1642050369912/s2LP3SjLt.png)

All backups should be kept in a secure manner to avoid unauthorized access or use. This should include encryption and/or a password as applicable. Regardless of where you store your backup, it's important that only authorized individuals have access to the data on that backup and be able to restore the backup in a disaster. 

### Backup Strategies 
A backup strategy is a written plan for your business that details the backup plan for the business. This plan will consist of the systems and/or data to be backed up, the frequency of the backup, the duration of how long the backup will be stored, the recovery point objective and the recovery time object.  This backup strategy also needs to list the responsible parties for administering the backup strategy, responsible users or departments, and any third-party company's and/or software solutions involved. 

Recovery Point Objective (RPO)
The RPO refers to the point in time that you are able to recover the backup. This is the point in history where you are able to restore your data, losing anything that happened after that backup until present time. 

If you are doing manual backups every week, then your RPO is up to 1 week. If it's currently Thursday and your last backup is Friday then you may lose any data that happened after Friday. This may be acceptable to some businesses. If the backup from last Friday doesn't work then you would have to go back another full week to recover from the previous Friday. 

If you are doing incremental backups every hour, 24x7, then your recovery point objective is 1 hour, meaning the most data that should be lost is up to the last hour of work. This also provides multiple recovery points in history that you could restore back 3 hours ago or even 18 hours ago if needed. This is useful in some virus infection scenarios where you want to restore to a specific time before the infection occurred. 

Recovery Time Objective (RTO)
RTO is the duration that your business can be down before it begins to negatively impact the business and/or it's customers. If your business is critical in nature and cannot be done for more than 1 hour, then your RTO is 1-hour. This is the time that you will have to get the business back up and running using your disaster recovery strategy in the event of a disaster. Some businesses may be just fine being closed for 1 day or more during a disaster while others cannot survive being closed for an hour. 

I worked with a medium-sized medical firm to establish a backup and disaster recovery plan where one previously didn't exist. The RTO for this business was set at 3 hours, meaning that the business could be down for as much as 3 hours without causing a major disruption to the healthcare operation. The RPO was set at 1-hour, meaning that we took full incremental backups every 60 minutes to ensure that in the RTO we had 3 points of recovery. 

### The Number 1 Rule with Backups
**You must test your backups!** Regardless of the backup media, the backup strategy, or the frequency, you must create a routine backing testing strategy. 


![A chalkboard with the word test written in all caps](https://cdn.hashnode.com/res/hashnode/image/upload/v1642051677646/bll62_6eE.jpeg)

Testing should be documented and confirmed by more than one person for accountability, or a screenshot saved with documentation showing proof of valid backups. **If you are using a backup and not testing that backup, there's a 50% chance when you need to use it, that backup will not be valid.** If you are paying another company to handle backups on your behalf, then you should be requesting proof that your backups are being tested at the interval agreed to between you and the backup company, but should be no less than quarterly. 

Methods for testing: 
There are several methods for testing that we will cover. The method you choose will depend on the type of backup that you are using. 

**Manual Backups**
Manual backups are tested by verifying the data exists and is accessible both on the source computer and another computer. If you backup to an external drive or a USB storage device, plug this device into another computer and make sure you can see the data on your drive. It’s also a good idea to make sure you can open and read the data. 

**Incremental/Differential Backups**
Incremental and Differential backups can generally be tested in a variety of ways. They can be tested by mounting the backup, which is a method of exploring the content in the backup chain and viewing files on it. Some incremental backup providers also allow you to virtual boot them as a method of testing that the system being backed up can be ran as a virtual machine. With cloud providers they provide a way of viewing the files contained in the backup and usually allow viewing different points in time along the chain. The key to this backup type is being able to go back in time and pick a specific date and/or time to restore from and browse files. 

### Disaster Recovery 
Disaster Recovery involves a written plan of action for how the business will respond to and handle different disaster scenarios. This is a rather lengthy document and involves multiple members of the organization. The restoration of backups is a small piece of a disaster recovery but a critical piece of it. Disaster recovery planning will involve how to recover the entire business in the event of a disaster. 

The easy scenario to consider is building destruction. Let's say the business is a small accounting firm with 30 employees and a lot of critical business data, they are using a cloud backup service and running incremental backups every hour. In the middle of the night when no one was in the building, there is an earthquake and the building has been destroyed. The disaster recovery plan for this accounting firm will need to include details such as where and how will the employees work, what equipment the staff will need to get up and running, contact lists for all vendors and employees since they will need to be notified of the disaster and what the business is doing in the recovery phase. The plan will also include how their offsite backup will be used to recover the data along with the specific steps, account information, encryption password, and how the employees will connect to restored data. 

## Conclusion
The purpose of this article was to make you familiar with the process of backing up and planning for disaster recovery, including the types of backups and methods for storing the backup. To provide a proper backup solution for a business, you must consider several key factors about the business to have an effective strategy. The stakeholders of the business need to be involved in some of the decision making to ensure the strategy is aligned with the business objectives. 

There is no one-size fits all backup strategy that will apply to every business. A good understanding of the business itself and the variety of options available will be a good start. A full disaster recovery plan should be documented, tested, and distributed to the disaster recovery team members. 

If you don't have a backup plan in place, or don't have proof of recent testing of your backups, then it's time to reach out to your IT Department or Backup vendor and get it started today. 

If you or your business is in need of a disaster recovery plan, recommendations, or a backup solution, please reach out to me through my business,  [Taco-IT](https://taco-it.com) or on  [Twitter](https://twitter.com/_ChrisBenjamin). This article is not intended as a sales pitch for my services, but I strongly feel that every business should have a backup in place. 

I’m on @buymeacoffee. If you like my work, you can buy me a taco and share your thoughts 🎉🌮
<a href="https://www.buymeacoffee.com/ChrisBenjamin" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" alt="Buy Me A Coffee" style="height: 60px !important;width: 217px !important;" ></a>