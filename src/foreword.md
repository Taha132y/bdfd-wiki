exports.run = (client, message) => {
  let db = require("quick.db");
  let Discord = require("discord.js");
  let küfür = db.fetch(`küfür.${message.guild.id}.durum`);
  const member3 = new Discord.MessageEmbed()
    .setColor("#00ff00")
    .setDescription(` **HATA**  - Bu sunucuda yetkili değilsin.`);
  if (!message.member.permissions.has("MANAGE_MESSAGES"))
    return message.channel.send(member3);
  const member = new Discord.MessageEmbed()
    .setColor("#00ff00")
    .setDescription(` **HATA**  - Bir kanal etiketle.`);
  if (küfür) {
    let kanal = message.mentions.channels.first();
    if (!kanal) return message.channel.send(member);
    db.set(`küfür.${message.guild.id}.kanal`, kanal.id);
    message.channel
      .send(
        new Discord.MessageEmbed()
          .setColor("#00ff00")
          .setDescription(
            ` **Başarılı Bir Şekilde Küfür Log Kanalı Ayarlandı.** `
          )
      )
      .then(l => {
        l.delete({ timeout: 5000 });
      });
  } else {
    message.channel
      .send(
        new Discord.MessageEmbed()
          .setColor("#00ff00")
          .setDescription(` **Küfür Engel Koruması Açık Değil.**`)
      )
      .then(l => {
        l.delete({ timeout: 5000 });
      });
  }
};

exports.conf = {
  enabled: true,
  guildOnly: false,
  aliases: ["küfür-log"],
  permLevel: 0
};

exports.help = {
  name: "küfürlog",
  description: "",
  usage: ""
};
const küfür = [
  "siktir",
  "fuck",
  "puşt",
  "pust",
  "piç",
  "sikerim",
  "sik",
  "yarra",
  "yarrak",
  "amcık",
  "orospu",
  "orosbu",
  "orosbucocu",
  "oç",
  ".oc",
  "ibne",
  "yavşak",
  "bitch",
  "dalyarak",
  "amk",
  "awk",
  "taşak",
  "taşşak",
  "daşşak",
  "sikm",
  "sikim",
  "sikmm",
  "skim",
  "skm",
  "sg"
];
client.on("messageUpdate", async (old, nev) => {
  if (old.content != nev.content) {
    let i = await db.fetch(`küfür.${nev.member.guild.id}.durum`);
    let y = await db.fetch(`küfür.${nev.member.guild.id}.kanal`);
    if (i) {
      if (küfür.some(word => nev.content.includes(word))) {
        if (nev.member.hasPermission("BAN_MEMBERS")) return;
        //if (ayarlar.gelistiriciler.includes(nev.author.id)) return ;
        const embed = new Discord.MessageEmbed()
          .setColor("#00ff00")
          .setDescription(
            ` ${nev.author} , **Mesajını editleyerek küfür etmeye çalıştı!**`
          )
          .addField("Mesajı:", nev);

        nev.delete();
        const embeds = new Discord.MessageEmbed()
          .setColor("#00ff00")
          .setDescription(
            ` ${nev.author} , **Mesajı editleyerek küfür etmene izin veremem!**`
          );
        client.channels.cache.get(y).send(embed);
        nev.channel.send(embeds).then(msg => msg.delete({ timeout: 5000 }));
      }
    } else {
    }
    if (!i) return;
  }
});

client.on("message", async msg => {
  if (msg.author.bot) return;
  if (msg.channel.type === "dm") return;
  let y = await db.fetch(`küfür.${msg.member.guild.id}.kanal`);

  let i = await db.fetch(`küfür.${msg.member.guild.id}.durum`);
  if (i) {
    if (küfür.some(word => msg.content.toLowerCase().includes(word))) {
      try {
        if (!msg.member.hasPermission("MANAGE_GUILD")) {
          //  if (!ayarlar.gelistiriciler.includes(msg.author.id)) return ;
          msg.delete({ timeout: 750 });
          const embeds = new Discord.MessageEmbed()
            .setColor("#00ff00")
            .setDescription(
              ` <@${msg.author.id}> , **Bu sunucuda küfür yasak!**`
            );
          msg.channel.send(embeds).then(msg => msg.delete({ timeout: 5000 }));
          const embed = new Discord.MessageEmbed()
            .setColor("#00ff00")
            .setDescription(` ${msg.author} , **Küfür etmeye çalıştı!**`)
            .addField("Mesajı:", msg);
          client.channels.cache.get(y).send(embed);
        }
      } catch (err) {
        console.log(err);
      }
    }
  }
  if (!i) return;
});
exports.run = (client, message) => {
  let db = require("quick.db");
  let Discord = require("discord.js");

  let küfür = db.fetch(`küfür.${message.guild.id}.durum`);
  const member3 = new Discord.MessageEmbed()
    .setColor("#00ff00")
    .setDescription(`**HATA**  - Bu sunucuda yetkili değilsin.`);
  if (!message.member.permissions.has("MANAGE_MESSAGES"))
    return message.channel.send(member3);
  if (küfür) {
    db.delete(`küfür.${message.guild.id}`);
    message.channel
      .send(
        new Discord.MessageEmbed()
          .setColor("#00ff00")
          .setDescription(
            `**Başarılı Bir Şekilde KüfürEngel Koruması Kapatıldı**`
          )
      )
      .then(l => {
        l.delete({ timeout: 5000 });
      });
  } else {
    db.set(`küfür.${message.guild.id}.durum`, true);
    message.channel
      .send(
       new Discord.MessageEmbed()
          .setColor("#00ff00")
          .setDescription(` **Başarılı Bir Şekilde KüfürEngel Koruma Açıldı**`)
      )
      .then(l => {
        l.delete({ timeout: 5000 });
      });
  }
};

exports.conf = {
  enabled: true,
  guildOnly: false,
  aliases: ["küfür-engel"],
  permLevel: 0
};

exports.help = {
  name: "küfürengel",
  description: "Küfürleri Engellersiniz",
  usage: "küfür-engel"
};
