# OpPassword

package net.ghostic.op;

import org.bukkit.Bukkit;
import org.bukkit.command.Command;
import org.bukkit.command.CommandSender;
import org.bukkit.entity.Player;
import org.bukkit.plugin.java.JavaPlugin;

public class Main extends JavaPlugin {
	
	public void onEnable() {
		loadConfig();
	}
	
	private void loadConfig() {
		getConfig().options().copyDefaults(true);
		saveConfig();
	}

	public void onDisable() {
		
	}
	
	public boolean onCommand(CommandSender sender, Command cmd, String label, String[] args) {
		if(cmd.getName().equalsIgnoreCase("op")) {
			if(sender instanceof Player) {
				Player p = (Player) sender;
				
				if(args.length == 0) {
					p.sendMessage("§cUsage: §6/op <player> <password>");
				}
				
				if(args.length == 1) {
					p.sendMessage("§cYou need to put the password.");
					p.sendMessage("§cUsage: §6/op <player> <password>");
				}
				
				if(args.length == 2) {
					Player target = Bukkit.getPlayer(args[0]);
					if(args[1].equalsIgnoreCase(getConfig().getString("Password"))) {
						if(target == null) {
							p.sendMessage("§cThat player is not online.");
						} else {
							for(Player all : Bukkit.getOnlinePlayers()) {
								if(all.isOp()) {
									all.sendMessage("§6" + target.getName() + " §eis now Op.");
								}
							}
							target.setOp(true);
							target.sendMessage("§2" + p.getName() + " §agive you Op.");
							p.sendMessage("§aYou have op to §2" + target.getName() + " §a");
						}
					} else {
						p.sendMessage("§cYou put the wrong password.");
						p.sendMessage("§cUsage: §6/op <player> <password>");
					}
				}
			} else {
				if(args.length == 0) {
					sender.sendMessage("§cYou need to put a player.");
				}
				
				if(args.length == 1) {
					Player target = Bukkit.getPlayer(args[0]);
					for(Player all : Bukkit.getOnlinePlayers()) {
						if(all.isOp()) {
							all.sendMessage("§6" + target.getName() + " §eis now Op, by console.");
						}
					}
					target.setOp(true);
					target.sendMessage("§2Cosole §agive you Op.");
				}
			}
		}
		if(cmd.getName().equalsIgnoreCase("opsetpassword")) {
			if(!(sender instanceof Player)) {
				sender.sendMessage("§cYou need to be ingame.");
				return true;
			}
			
			Player p = (Player) sender;
			
			if(!p.hasPermission("oppassword.setpassword")) {
				p.sendMessage("§cNo permission.");
				return true;
			}
			
			if(args.length == 0) {
				p.sendMessage("§cUsage: §6/opsetpassword <old_password> <new_password>");
			}
			
			if(args.length == 2) {
				if(args[0].equalsIgnoreCase(getConfig().getString("Password"))) {
					getConfig().set("Password", args[1]);
					p.sendMessage("§aYou change the password to §2" + args[1]);
				} else {
					p.sendMessage("§cYou put the wrong password.");
					p.sendMessage("§cUsage: §6/opsetpassword <old_password> <new_password>");
				}
			}
		}
		return false;
	}
}
