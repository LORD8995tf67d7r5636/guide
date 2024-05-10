```js
const { Client, GatewayIntentBits, ActivityType, EmbedBuilder,TextInputBuilder, ActionRowBuilder,ModalBuilder,ButtonBuilder, ButtonStyle, TextInputStyle } = require("discord.js");
const client = new Client({
    intents: [
        GatewayIntentBits.Guilds,
        GatewayIntentBits.GuildMembers,
        GatewayIntentBits.GuildMessages,
        GatewayIntentBits.MessageContent
    ]
});

client.on("ready", () => {
    console.log(`${client.user.username} Is Ready`)
    client.user.setActivity(`+setup`, { type: ActivityType.Playing })
});

 client.login("");// token is here ok !?

 process.on('unhandledRejection', error => console.log(error));
 process.on('uncaughtException', error => console.log(error));



client.on("messageCreate", async message => {
    if(message.content === "+setup"){
        const allowedRole = message.guild.roles.cache.get('1139844965996957716');
        if (!allowedRole || !message.member.roles.cache.has(allowedRole.id)) {
            return message.reply('**عذراً ، لست من مسؤولين الادارة لكي تستخدم الأمر.**');
        }
const row = new ActionRowBuilder()
.addComponents(
new ButtonBuilder()
    .setCustomId("log")
    .setLabel("تسجيل دخول")
    .setStyle(ButtonStyle.Secondary),
    new ButtonBuilder()
    .setCustomId("leave")
    .setLabel("تسجيل خروج")
    .setStyle(ButtonStyle.Secondary)
)
message.channel.send({ components: [row] })
}
});



client.on('interactionCreate', async interaction => {
if(!interaction.isButton()) return;
    if(interaction.customId == "log"){
const channel = client.channels.cache.get('1163880474846953523');
        
        
        
        const embed = new EmbedBuilder()
        .setDescription(`**تسجيل دخول\n \nالشخص:\n${interaction.user}\n \nالوقت:\n${new Date().toLocaleString()}**`)
        .setAuthor({ name : interaction.user.username, iconURL: interaction.user.displayAvatarURL({ dynamic: true })})
        .setThumbnail(interaction.guild.iconURL({ dynamic: true }))
        await interaction.reply({ content: "تم تسجيل دخولك بنجاح", ephemeral: true })
        channel.send({ embeds: [embed] })
        
} else if(interaction.customId == "leave"){   
    
    const modal = new ModalBuilder()
            .setCustomId('send')
            .setTitle('التقرير اليومي');

        const tokenBot = new TextInputBuilder()
            .setCustomId('leaves')
            .setLabel("تقرير الخروج")
            .setMinLength(1)
            .setMaxLength(3000)
            .setPlaceholder("أكتب تقريرك هنا")
            .setStyle(TextInputStyle.Short);
    const rows = [
            new ActionRowBuilder().addComponents(tokenBot) ]
        
        modal.addComponents(...rows);
        interaction.showModal(modal);
}
});




client.on('interactionCreate', async modal => {
    if(!modal.isModalSubmit()) return;
    if(modal.customId == "send") {
     const channel = client.channels.cache.get("1163880474846953523");
        
       
        
     const leaves = modal.fields.getTextInputValue("leaves")
     const embed = new EmbedBuilder()
        .setDescription(`**تسجيل خروج\n \nالشخص:${modal.user}\n \nالوقت:\n${new Date().toLocaleString()}\n\nالتقرير:\n${leaves}**`)
        .setAuthor({ name: modal.user.username,iconURL: modal.user.displayAvatarURL({ dynamic: true })})
        .setThumbnail(modal.guild.iconURL({ dynamic: true }))
     modal.reply({content:"تم تسجيل خروجك بنجاح" , ephemeral:true})
        
     channel.send({ embeds: [embed] })
   }
});
 

client.login("")
```
