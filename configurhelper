const botconfig = require("./botconfig.json");
const Discord = require("discord.js");

const bot = new Discord.Client();

bot.on("ready", async () => {
 console.log(`${bot.user.username} is online!`);
 bot.user.setActivity("Your Activity", {type: "WATCHING"});
});

bot.on("message", async message => {
    if(message.author.bot) return;
    if(message.channel.type === "dm") return;

    let prefix = botconfig.prefix;
    let messageArray = message.content.split(" ");
    let cmd = messageArray[0];
    let args = messageArray.slice(1);

    //Commands:
 const ms = require("ms");
if(cmd === `${prefix}help`){
    let helpEmbed = new Discord.RichEmbed()
    .setDescription("Kick")
    .setColor("#FEAAF4")
    .addField("Prefix", ` ${prefix}  `)
    .addField("(Public Command)report", "report **@User** 'Reason' ")
    .addField("(User With MANAGE_MESSAGE Permission)kick", "kick **@User** 'Reason' ")
    .addField("(User With MANAGE_MESSAGE Permission)ban", "ban **@User** 'Reason' ")
    .addField("(User With MANAGE_MESSAGE Permission)tempmute", "tempmute **@User** 'Time' **Example:**`1s OR 1m OR 1h OR 1d` ");

    message.channel.send(helpEmbed)
}

    if(cmd === `${prefix}tempmute`){

        let tomute = message.guild.member(message.mentions.users.first() || message.guild.members.get(args[0]));
        if(!tomute) return message.reply("**Error: **`User Not Found.`");
        if(!message.member.hasPermission("MANAGE_MESSAGES")) return message.channel.send("**Error: **`You Need MANAGE_MESSAGES Permission To Use This Command");
        if(tomute.hasPermission("MANAGE_MESSAGES")) return message.channel.send("**Error: **`User Cannot Be Muted`")
        let muterole = message.guild.roles.find(`name`,`muted`);

        //Create "Muted" Role
        if(!muterole){
            try{
               muterole = await message.guild.createRole({
                 name: "Muted",
                 color: "#000000",
                 permission:[]
                })
            message.guild.channels.forEach(async (channel, id)=> {
                await channel.overwritePermissions(muterole, {
                 SEND_MESSAGES: false,
                 ADD_REACTIONS: false
                });
            });
            }catch(e){
                console.log(e.stack);
            }
        }
       let mutetime = args[1];
       if (!mutetime) return message.reply("**Error: **`Bad Usage: '/tempmute' @User#???? time`");

       await(tomute.addRole(muterole.id));
       message.reply(`<@${tomute.id}> Has Been Muted For ${ms(ms(mutetime))}`);
    setTimeout(function(){
      tomute.removeRole(muterole.id)
      message.channel.send(`<@${tomute.id}> Has Been Unmuted `);
    }, ms(mutetime));
       
    }
  //End Of "Muted" Role
    if(cmd === `${prefix}ban`){
        let bUser = message.guild.member(message.mentions.users.first() || message.guild.members.get(args[0]));
        if(!bUser) return message.channel.send("**Error: **`User Not Found.`");
        let bReason = args.join(" ").slice(22);
        if(!message.member.hasPermission("MANAGE_MESSAGES")) return message.channel.send("**Error: **`You Need MANAGE_MESSAGES Permission To Use This Command");
        if(bUser.hasPermission("MANAGE_MESSAGES")) return message.channel.send("**Error: **`User Cannot Be Banned, Try Again Later.`");
    
        let banEmbed = new Discord.RichEmbed()
        .setDescription("Ban")
        .setColor("#FEAAF4")
        .addField("Banned User", `${bUser} with ID ${bUser.id}`)
        .addField("Banned By", `<@${message.author.id}> with ID ${message.author.id}`)
        .addField("Banned In", message.channel)
        .addField("Reason", bReason);

        let alogs = message.guild.channels.find(`name`,"logs")
        if(!alogs) return message.channel.send("**Error: ** `Couldn't Find Logs.`");

        message.guild.member(bUser).ban(bReason);
        alogs.send(banEmbed);

    }

    if(cmd === `${prefix}kick`){
        let kUser = message.guild.member(message.mentions.users.first() || message.guild.members.get(args[0]));
        if(!kUser) return message.channel.send("**Error: **`User Not Found.`");
        let kReason = args.join(" ").slice(22);
        if(!message.member.hasPermission("MANAGE_MESSAGES")) return message.channel.send("**Error: **`You Need MANAGE_MESSAGES Permission To Use This Command");
        if(kUser.hasPermission("MANAGE_MESSAGES")) return message.channel.send("**Error: **`User Cannot Be Kicked, Try Again Later.`");
        
        let kickEmbed = new Discord.RichEmbed()
        .setDescription("Kick")
        .setColor("#FEAAF4")
        .addField("Kicked User", `${kUser} with ID ${kUser.id}`)
        .addField("Kicked By", `<@${message.author.id}> with ID ${message.author.id}`)
        .addField("Kicked In", message.channel)
        .addField("Reason", kReason);

        let alogs = message.guild.channels.find(`name`,"logs")
        if(!alogs) return message.channel.send("**Error: ** `Couldn't Find Channel For Kicked Players.`");
        message.guild.member(kUser).kick(kReason)
        alogs.send(kickEmbed)

    }


    if(cmd === `${prefix}report`){

        let rUser = message.guild.member(message.mentions.users.first() || message.guild.members.get(args[0]));
        if(!rUser) return message.channel.send("User Not Found.");
        let reason = args.join(" ").slice(22);

        let reportEmbed = new Discord.RichEmbed()
        .setDescription("Reports")
        .setColor("#FEAAF4")
        .addField("Reported Users", `${rUser} with ID: ${rUser.id}`)
        .addField("Reporter", `${message.author} with ID ${message.author.id}`)
        .addField("Reported In", message.channel)
        .addField("Report Reason", reason);

        let reportschannel = message.guild.channels.find(`name`,"reports")
        if(!reportschannel) return message.channel.send("**Error: ** `Couldn't Find Channel For Reports.`");


        message.delete().catch(O_o=>{});
        reportschannel.send(reportEmbed)
        return;
    }

    if(cmd === `${prefix}helper`){
        return message.channel.send("**Bot: `Online`**");
    }    

    if(cmd === `${prefix}serverinfo`){

        let sicon = message.guild.iconURL
        let serverembed = new Discord.RichEmbed()
        .setDescription("Server Information")
        .setColor("#FEAAF4")
        .setThumbnail(sicon)
        .addField("Server Name", message.guild.name)
        .addField("Server Owner", message.guild.owner)
        .addField("Total Members",  message.guild.memberCount)
    
        return message.channel.send(serverembed)
    }
    if(cmd === `${prefix}info`){

        let bicon = bot.user.displayAvatarURL;
        let botembed = new Discord.RichEmbed()

        .setDescription("Configur Information")
        .setColor("#FEAAF4")
        .setThumbnail(bicon)
        .addField("Configur Helper", "Discord Server Administrator Tool")
        .addField("Bot Created By: ", "Arkhie#8479");      
        return message.channel.send(botembed);
    }
});

bot.login(process.env.BOT_TOKEN);

