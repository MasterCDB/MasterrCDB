public OnPlayerCommandText(playerid, cmdtext[])
{
	new cmd[256], idx, string[256];

	if(PlayerInfo[playerid][Logged] == 0)
	{
		SendClientMessage(playerid, Vermelho, "Logue-se antes.");
		return 1;
	}

	if(GetTickCount() > FloodTimer[playerid])
	{
		FloodAlert[playerid] = 0;
	}

	FloodTimer[playerid] = GetTickCount() +TimerFlood;
	FloodAlert[playerid]++;

	if(FloodAlert[playerid] > 1 && FloodAlert[playerid] < AlertFlood-1)
	{
		format(string, sizeof(string), "|_ ANTI-FLOOD _| Você tem %d/%d avisos.", FloodAlert[playerid], AlertFlood);
		SendClientMessage(playerid, Amarelo, string);
	}
	else if(FloodAlert[playerid] == AlertFlood-1)
	{
		format(string, sizeof(string), "|_ ANTI-FLOOD _| Você tem %d/%d avisos. Mais um e você será kickado.", FloodAlert[playerid], AlertFlood);
		SendClientMessage(playerid, Amarelo, string);
	}
	else if(FloodAlert[playerid] == AlertFlood)
	{
		format(string, sizeof(string), "O(A) jogador(a) %s foi kickado(a) por DUBAYBot. Motivo: Flood Comando", GetPlayerNameEx(playerid));
		SendClientMessageToAll(Amarelo, string);
		KickLog(string);
		Kick(playerid);
		return 1;
	}

	cmd = strtok(cmdtext, idx);
	printf("[CMD] %s (ID: %d) digitou o comando ( %s ).", GetPlayerNameEx(playerid), playerid, cmdtext);

	// Anti Rcon Hack
	if(strcmp("/scon", cmd, true) == 0)
	{
		new sconcommand[128];

		if(sscanf(cmdtext, "s[6]s[128]", cmd, sconcommand))
		{
			return 1;
		}
		if(strcmp("login", sconcommand, true, 5) == 0)
		{
			new sconpass[20];

			if(sscanf(cmdtext, "s[6]s[20]", cmd, sconpass))
			{
				return 1;
			}
			format(string, sizeof(string), "login %s", rconpass);
			if(strcmp(sconpass, string, true) == 0)
			{
				PlayerInfo[playerid][SCON] = true;
				SendClientMessage(playerid, COLOR_WHITE, "SCON: Você está logado como administrador.");
			}
			else
			{
				SendClientMessage(playerid, COLOR_WHITE, "scon: Senha digitada incorreta.");
			}
		}
		else if(strcmp("ban", sconcommand, true, 5) == 0)
		{
			new plid, motivo[64];

			if(sscanf(cmdtext, "s[4]us[64]", cmd, plid, motivo))
			{
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				if(IsPlayerNPC(plid))
				{
					SendClientMessage(playerid, Vermelho, "Você não pode fazer isso com um NPC.");
					return 1;
				}
				ClearChatbox(plid, 3);
				VBanID(playerid, plid, motivo);
			}
		}
		else if(strcmp("unban", sconcommand, true, 5) == 0)
		{
			new info[64];

			if(sscanf(cmdtext, "s[6]s[64]", cmd, info))
			{
				return 1;
			}
			VUnBan(playerid, info);
		}
		else if(PlayerInfo[playerid][SCON] == true)
		{
			SendRconCommand(sconcommand);

			format(string, sizeof(string), "SCON: RCON Comando \"%s\" enviado", sconcommand);
			SendClientMessage(playerid, COLOR_WHITE, string);
		}
		return 1;
	}

	for(new i = 0; i < MAX_PLAYERS; i++)
	{
		if(IsPlayerConnected(i))
		{
			if(vercmds[i] == 1)
			{
				new mntcmd[256];
				format(mntcmd, sizeof(mntcmd), "%s (ID: %d) digitou o comando ( %s ).", GetPlayerNameEx(playerid), playerid, cmdtext);
				SendClientMessage(i, Blue, mntcmd);
			}
		}
	}

	if(strcmp("/radar", cmd, true) == 0)
	{
		SendClientMessage(playerid, Vermelho, "***************************** SISTEMA DE RADAR *****************************");
		SendClientMessage(playerid, Azul, "Existe radar na prefeitura, hospital 1 e 2 e na biblioteca em Los Santos.");
		SendClientMessage(playerid, Azul, "O limite de velocidade é entre 80 KM/H, fique atento, olhe no velocimetro.");
		SendClientMessage(playerid, Azul, "Se você exceder o limite de velocidade nesses locais, levara multa de 500$.");
		SendClientMessage(playerid, Vermelho, "***************************** SISTEMA DE RADAR *****************************");
		return 1;
	}

	if(strcmp(cmd, "/props", true) == 0)
	{
		SendClientMessage(playerid, Azul, "/comprarprop: Compra uma propriedade.");
		SendClientMessage(playerid, Azul, "/venderprop: Vende uma propriedade.");
		SendClientMessage(playerid, Azul, "/sacarprop: Pega o dinhero da propriedade.");
		SendClientMessage(playerid, Azul, "/propnm [nome]: Muda o nome da sua prop.");
		SendClientMessage(playerid, Amarelo, "Todo dia as 15:00 as propriedades recebem renda.");
		return 1;
	}

	if(strcmp(cmd, "/casas", true) == 0)
	{
		SendClientMessage(playerid, Azul, "/comprarcasa: Compra uma casa.");
		SendClientMessage(playerid, Azul, "/vendercasa: Vende uma casa.");
		SendClientMessage(playerid, Azul, "/convidarcasa [id]: Convida alguém a morar na sua casa.");
		SendClientMessage(playerid, Azul, "/expulsarcasa [nick]: Expulsa um morador da sua casa.");
		return 1;
	}

	if(strcmp(cmd, "/portao", true) == 0)
	{
		SendClientMessage(playerid, Azul, "/ap [id]: Abre um determinado portão.");
		SendClientMessage(playerid, Azul, "/fp [id]: Fecha um determinado portão.");
		SendClientMessage(playerid, Azul, "/infoportao: Exibe ID e Dono do portão.");
		SendClientMessage(playerid, Azul, "/copiachave: Gera uma cópia da sua chave para alguem.");
		SendClientMessage(playerid, Azul, "/tomarchave: Toma a cópia da sua chave de alguem.");
		return 1;
	}

	if(strcmp(cmd, "/comandoslaser", true) == 0)
	{
		#if defined LaserUser
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "LaserP") == 1)
		{
			SendClientMessage(playerid, Azul, "/laseron: Liga a Mira Laser.");
			SendClientMessage(playerid, Azul, "/laseroff: Desliga a Mira Laser.");
			SendClientMessage(playerid, Azul, "/lasercol [red/blue/pink/orange/green/yellow]");
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem Mira Leser, compre na loja de utilidades.");
		}
		#else
		SendClientMessage(playerid, Azul, "Comando desativado temporáriamente.");
		#endif

		return 1;
	}

	if(strcmp("/laseron", cmd, true) == 0)
	{
		#if defined LaserUser
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "LaserP") == 1)
		{
			SetPVarInt(playerid, "laser", 1);
			SetPVarInt(playerid, "color", GetPVarInt(playerid, "color"));
			SendClientMessage(playerid, Verde, "Mira Laser on!");
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem Mira Leser, compre na loja de utilidades.");
		}
		#else
		SendClientMessage(playerid, Azul, "Comando desativado temporáriamente.");
		#endif

		return 1;
	}

	if(strcmp("/laseroff", cmd, true) == 0)
	{
		#if defined LaserUser
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "LaserP") == 1)
		{
			SetPVarInt(playerid, "laser", 0);
			RemovePlayerAttachedObject(playerid, 0);
			SendClientMessage(playerid, Verde, "Mira Laser off!");
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem Mira Leser, compre na loja de utilidades.");
		}
		#else
		SendClientMessage(playerid, Azul, "Comando desativado temporáriamente.");
		#endif

		return 1;
	}

	if(strcmp("/lasercol", cmd, true) == 0)
	{
		#if defined LaserUser
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "LaserP") == 1)
		{
			new cor[16];

			if(sscanf(cmdtext, "s[10]s[16]", cmd, cor))
			{
				SendClientMessage(playerid, 0x00E800FF, "Use: /lasercol [red/blue/pink/orange/green/yellow]");
				return 1;
			}
			if(!strcmp(cor, "red", true))
			{
				SetPVarInt(playerid, "color", 18643);
			}
			else if(!strcmp(cor, "blue", true))
			{
				SetPVarInt(playerid, "color", 19080);
			}
			else if(!strcmp(cor, "pink", true))
			{
				SetPVarInt(playerid, "color", 19081);
			}
			else if(!strcmp(cor, "orange", true))
			{
				SetPVarInt(playerid, "color", 19082);
			}
			else if(!strcmp(cor, "green", true))
			{
				SetPVarInt(playerid, "color", 19083);
			}
			else if(!strcmp(cor, "yellow", true))
			{
				SetPVarInt(playerid, "color", 19084);
			}
			else SendClientMessage(playerid, 0x00E800FF, "Cor inválida!");
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem mira leser, compre na loja de utilidades.");
		}
		#else
		SendClientMessage(playerid, Azul, "Comando desativado temporáriamente.");
		#endif

		return 1;
	}

	if(strcmp("/dance", cmd, true) == 0)
	{
		if(GetPlayerState(playerid) == PLAYER_STATE_ONFOOT)
		{
			new animationplayed;

			if(sscanf(cmdtext, "s[7]d", cmd, animationplayed))
			{
				SendClientMessage(playerid, Vermelho, "Use /dance [1-3]");
				return 1;
			}
			if(animationplayed < 1 || animationplayed > 3)
			{
				SendClientMessage(playerid, Vermelho, "Use /dance [1-3]");
				return 1;
			}
			if(animationplayed == 1)
			{
				SetPlayerSpecialAction(playerid, SPECIAL_ACTION_DANCE1);
			}
			else if(animationplayed == 2)
			{
				SetPlayerSpecialAction(playerid, SPECIAL_ACTION_DANCE2);
			}
			else if(animationplayed == 3)
			{
				SetPlayerSpecialAction(playerid, SPECIAL_ACTION_DANCE3);
			}
		}
		return 1;
	}

	if(strcmp("/fumar", cmd, true) == 0)
	{
		SetPlayerSpecialAction(playerid, SPECIAL_ACTION_SMOKE_CIGGY);
		return 1;
	}

	if(strcmp("/bebado", cmd, true) == 0)
	{
		#if defined AnimLoopsUser
		LoopingAnim(playerid, "PED", "WALK_DRUNK", 4.1, 0, 1, 1, 1, 1);
		#else
		ApplyAnimation(playerid, "PED", "WALK_DRUNK", 4.1, 0, 1, 1, 1, 1);
		#endif

		return 1;
	}
	if(strcmp("/merda", cmd, true) == 0)
	{
		#if defined AnimLoopsUser
		LoopingAnim(playerid, "RAPPING", "Laugh_01", 4.1, 0, 1, 1, 1, 1);
		#else
		ApplyAnimation(playerid, "RAPPING", "Laugh_01", 4.1, 0, 1, 1, 1, 1);
		#endif

		return 1;
	}
	if(strcmp("/mascararse", cmd, true) == 0)
	{
		#if defined AnimLoopsUser
		LoopingAnim(playerid, "SHOP", "ROB_Shifty", 4.1, 0, 1, 1, 1, 1);
		#else
		ApplyAnimation(playerid, "SHOP", "ROB_Shifty", 4.1, 0, 1, 1, 1, 1);
		#endif

		return 1;
	}
	if(strcmp("/cruzarb", cmd, true) == 0)
	{
		#if defined AnimLoopsUser
		LoopingAnim(playerid, "COP_AMBIENT", "Coplook_loop", 4.1, 0, 1, 1, 1, 1);
		#else
		ApplyAnimation(playerid, "COP_AMBIENT", "Coplook_loop", 4.1, 0, 1, 1, 1, 1);
		#endif

		return 1;
	}
	if(strcmp("/deitar", cmd, true) == 0)
	{
		#if defined AnimLoopsUser
		LoopingAnim(playerid, "BEACH", "bather", 4.1, 0, 1, 1, 1, 1);
		#else
		ApplyAnimation(playerid, "BEACH", "bather", 4.1, 0, 1, 1, 1, 1);
		#endif

		return 1;
	}
	if(strcmp("/abaixar", cmd, true) == 0)
	{
		#if defined AnimLoopsUser
		LoopingAnim(playerid, "PED", "cower", 4.1, 0, 1, 1, 1, 1);
		#else
		ApplyAnimation(playerid, "PED", "cower", 4.1, 0, 1, 1, 1, 1);
		#endif

		return 1;
	}
	if(strcmp("/vomitar", cmd, true) == 0)
	{
		#if defined AnimLoopsUser
		LoopingAnim(playerid, "FOOD", "EAT_Vomit_P", 4.1, 0, 1, 1, 1, 1);
		#else
		ApplyAnimation(playerid, "FOOD", "EAT_Vomit_P", 4.1, 0, 1, 1, 1, 1);
		#endif

		return 1;
	}
	if(strcmp("/rap", cmd, true) == 0)
	{
		#if defined AnimLoopsUser
		LoopingAnim(playerid, "ON_LOOKERS", "wave_loop", 4.1, 0, 1, 1, 1, 1);
		#else
		ApplyAnimation(playerid, "ON_LOOKERS", "wave_loop", 4.1, 0, 1, 1, 1, 1);
		#endif

		return 1;
	}
	if(strcmp("/cobrar", cmd, true) == 0)
	{
		#if defined AnimLoopsUser
		OnePlayAnim(playerid, "DEALER", "DEALER_DEAL", 4.1, 0, 1, 1, 1, 1);
		#else
		ApplyAnimation(playerid, "DEALER", "DEALER_DEAL", 4.1, 0, 1, 1, 1, 1);
		#endif

		return 1;
	}
	if(strcmp("/fumar2", cmd, true) == 0)
	{
		#if defined AnimLoopsUser
		LoopingAnim(playerid, "SMOKING", "F_smklean_loop", 4.1, 0, 1, 1, 1, 1);
		#else
		ApplyAnimation(playerid, "SMOKING", "F_smklean_loop", 4.1, 0, 1, 1, 1, 1);
		#endif

		return 1;
	}
	if(strcmp("/passarmao", cmd, true) == 0)
	{
		#if defined AnimLoopsUser
		OnePlayAnim(playerid, "SWEET", "sweet_ass_slap", 4.1, 0, 1, 1, 1, 1);
		#else
		ApplyAnimation(playerid, "SWEET", "sweet_ass_slap", 4.1, 0, 1, 1, 1, 1);
		#endif

		return 1;
	}
	if(strcmp("/sentar", cmd, true) == 0)
	{
		#if defined AnimLoopsUser
		LoopingAnim(playerid, "BEACH", "ParkSit_M_loop", 4.1, 0, 1, 1, 1, 1);
		#else
		ApplyAnimation(playerid, "BEACH", "ParkSit_M_loop", 4.1, 0, 1, 1, 1, 1);
		#endif

		return 1;
	}
	if(strcmp("/conversar", cmd, true) == 0)
	{
		#if defined AnimLoopsUser
		OnePlayAnim(playerid, "PED", "IDLE_CHAT", 4.1, 0, 1, 1, 1, 1);
		#else
		ApplyAnimation(playerid, "PED", "IDLE_CHAT", 4.1, 0, 1, 1, 1, 1);
		#endif

		return 1;
	}
	if(strcmp("/fodase", cmd, true) == 0)
	{
		#if defined AnimLoopsUser
		OnePlayAnim(playerid, "PED", "fucku", 4.1, 0, 1, 1, 1, 1);
		#else
		ApplyAnimation(playerid, "PED", "fucku", 4.1, 0, 1, 1, 1, 1);
		#endif

		return 1;
	}
	if(strcmp("/observar", cmd, true) == 0)
	{
		#if defined AnimLoopsUser
		LoopingAnim(playerid, "BAR", "dnk_stndF_loop", 4.1, 0, 1, 1, 1, 1);
		#else
		ApplyAnimation(playerid, "BAR", "dnk_stndF_loop", 4.1, 0, 1, 1, 1, 1);
		#endif

		return 1;
	}
	if(strcmp("/taichi", cmd, true) == 0)
	{
		#if defined AnimLoopsUser
		LoopingAnim(playerid, "PARK", "Tai_Chi_Loop", 4.1, 0, 1, 1, 1, 1);
		#else
		ApplyAnimation(playerid, "PARK", "Tai_Chi_Loop", 4.1, 0, 1, 1, 1, 1);
		#endif

		return 1;
	}
	if(strcmp("/strip", cmd, true) == 0)
	{
		#if defined AnimLoopsUser
		LoopingAnim(playerid, "STRIP", "STR_B2C", 4.1, 0, 1, 1, 1, 1);
		#else
		ApplyAnimation(playerid, "STRIP", "STR_B2C", 4.1, 0, 1, 1, 1, 1);
		#endif

		return 1;
	}
	if(strcmp("/mulhersexo", cmd, true) == 0)
	{
		#if defined AnimLoopsUser
		LoopingAnim(playerid, "benchpress", "gym_bp_up_B", 4.1, 0, 1, 1, 1, 1);
		#else
		ApplyAnimation(playerid, "benchpress", "gym_bp_up_B", 4.1, 0, 1, 1, 1, 1);
		#endif

		return 1;
	}
	if(strcmp("/comermulher", cmd, true) == 0)
	{
		#if defined AnimLoopsUser
		LoopingAnim(playerid, "MD_END", "END_SC1_SMO", 4.1, 0, 1, 1, 1, 1);
		#else
		ApplyAnimation(playerid, "MD_END", "END_SC1_SMO", 4.1, 0, 1, 1, 1, 1);
		#endif

		return 1;
	}
	if(strcmp("/mulherfudida", cmd, true) == 0)
	{
		#if defined AnimLoopsUser
		LoopingAnim(playerid, "FINALE", "FIN_Land_Die", 4.1, 0, 1, 1, 1, 1);
		#else
		ApplyAnimation(playerid, "FINALE", "FIN_Land_Die", 4.1, 0, 1, 1, 1, 1);
		#endif

		return 1;
	}
	if(strcmp("/de4", cmd, true) == 0)
	{
		#if defined AnimLoopsUser
		LoopingAnim(playerid, "FINALE", "FIN_Land_Car", 4.1, 0, 1, 1, 1, 1);
		#else
		ApplyAnimation(playerid, "FINALE", "FIN_Land_Car", 4.1, 0, 1, 1, 1, 1);
		#endif

		return 1;
	}
    if(strcmp("/compraradm", cmdtext, true, 10) == 0)
	{
		new strcmd[1000];
		strcat(strcmd, "» Preço Do ADM LV 1 10,00 Reais Por Mes !\n", sizeof(strcmd));
		strcat(strcmd, "» Preço Do ADM LV 2 15,00 Reais Por Mes !\n", sizeof(strcmd));
		strcat(strcmd, "» Preço Do ADM LV 3 20,00 Reais Por Mes !\n", sizeof(strcmd));
		strcat(strcmd, "» Preço Do ADM LV 4 25,00 Reais Por Mes !\n", sizeof(strcmd));
		strcat(strcmd, "» Preço Do ADM LV 5 30 Reais Por Mes !\n", sizeof(strcmd));
		strcat(strcmd, "» Para Mais Detalhes Adicione discord ou o discord Do Server!\n", sizeof(strcmd));
		strcat(strcmd, "» discord : Stremmer_Scripter#0961\n", sizeof(strcmd));
		strcat(strcmd, "» Discord server: https://discord.gg/k5JCCDxVAt\n", sizeof(strcmd));
		ShowPlayerDialog(playerid, playersimp, DIALOG_STYLE_MSGBOX, ".:: - Comandos Úteis - ::.", strcmd, "OK", "");
		return 1;
    }
    if(strcmp("/comprarvip", cmdtext, true, 10) == 0)
	{
		new strcmd[1000];
		strcat(strcmd, "» Preço Do vip por 5 dias 2,00 Reais Por Mes !\n", sizeof(strcmd));
		strcat(strcmd, "» Preço Do vip por 10 dias 5,00 Reais Por Mes !\n", sizeof(strcmd));
		strcat(strcmd, "» Preço Do vip por 15 dias 10,00 Reais Por Mes !\n", sizeof(strcmd));
		strcat(strcmd, "» Preço Do vip por 20 dias 15,00 Reais Por Mes !\n", sizeof(strcmd));
		strcat(strcmd, "» Preço Do vip por 25 dias 20,00 Reais Por Mes !\n", sizeof(strcmd));
		strcat(strcmd, "» Preço Do vip por 30 dias 30,00 Reais Por Mes !\n", sizeof(strcmd));
		strcat(strcmd, "» Para Mais Detalhes Adicione discord ou o discord Do Server!\n", sizeof(strcmd));
		strcat(strcmd, "» discord :Stremmer_Scripter#0961\n", sizeof(strcmd));
		strcat(strcmd, "» discord server : https://discord.gg/k5JCCDxVAt\n", sizeof(strcmd));
		ShowPlayerDialog(playerid, playersimp, DIALOG_STYLE_MSGBOX, ".:: - Comandos Úteis - ::.", strcmd, "OK", "");
		return 1;
    }
    if(strcmp("/animes", cmd, true) == 0)
	{
		SendClientMessage(playerid, Blue, "====================== Lista de Animes ======================");
		SendClientMessage(playerid, Verde, "/bebado /merda /mascararse /cruzarb /deitar /sentar");
		SendClientMessage(playerid, Verde, "/vomitar /rap /cobrar /fumar /passarmao /taichi");
		SendClientMessage(playerid, Verde, "/observar /fodase /conversar /abaixar /strip /fumar2");
		SendClientMessage(playerid, Verde, "/de4 /mulhersexo /comermulher /mulherfudida");
		SendClientMessage(playerid, Blue, "====================== Lista de Animes ======================");
		return 1;
	}

	if(strcmp("/gps", cmd, true) == 0)
	{
		#if defined FGPSUser
		if(!IsPlayerInAnyVehicle(playerid))
		{
			SendClientMessage(playerid, 0xE60000FF, "Você precisa estar em um veículo!");
			return 1;
		}
		if(GetPVarInt(playerid, "YEAH") == 1)
		{
			SendClientMessage(playerid, -1, "{E60000}Desligue o GPS antes {00CCFA}/gpsoff {E60000}para poder usar novamente.");
			return 1;
		}
		ShowPlayerDialog(playerid, GPSMenu, DIALOG_STYLE_LIST, "GPS", "Delegacias\nPrefeituras\nLojas e Diversos\nIgrejas\nHoteis\nHospitais\nUniversidades\nLanchonetes\nLoja de Acessórios", "Selecionar", "Cancelar");
		#else
		SendClientMessage(playerid, Azul, "Comando desativado temporáriamente.");
		#endif

		return 1;
	}

	if(strcmp("/gpsoff", cmd, true) == 0)
	{
		#if defined FGPSUser
		if(GetPVarInt(playerid, "YEAH") == 0)
		{
			SendClientMessage(playerid, -1, "{E60000}O GPS já está {00FF15}desligado{E60000}!");
			return 1;
		}
		DestroyDynamicRaceCP(PlayerInfo[playerid][FGPS_RCP]);
		DestroyDynamicObject(PlayerInfo[playerid][FGPSObject]);

		SetPVarInt(playerid, "YEAH", 0);
		DeletePVar(playerid, "Spongebob");
		DeletePVar(playerid, "Mario");
		DeletePVar(playerid, "SpiderPig");
		DeletePVar(playerid, "FAIL");

			#if defined UseTd
		PlayerTextDrawHide(playerid, PlayerInfo[playerid][F_GPSTD]);
		PlayerTextDrawDestroy(playerid, PlayerInfo[playerid][F_GPSTD]);
		PlayerInfo[playerid][F_GPSTD] = PlayerText:INVALID_TEXT_DRAW;
			#endif

		SendClientMessage(playerid, 0xFFFFFFFF, "Você desligou o GPS!");
		#else
		SendClientMessage(playerid, Azul, "Comando desativado temporáriamente.");
		#endif

		return 1;
	}

	if(strcmp(cmd, "/salvarspawn", true) == 0)
	{
		if(pAdmin[playerid] == 5)
		{
			new Float:X, Float:Y, Float:Z, Float:A, InteriorSpawn,
				VirtualWorld, comment[64], sstring[256], File:hFile;

			GetPlayerPos(playerid, X, Y, Z);
			GetPlayerFacingAngle(playerid, A);
			InteriorSpawn = GetPlayerInterior(playerid);
			VirtualWorld = GetPlayerVirtualWorld(playerid);

			if(sscanf(cmdtext, "s[13]ffffdds[64]", cmd, X, Y, Z, A, InteriorSpawn, VirtualWorld, comment))
			{
				format(sstring, sizeof(sstring), "%.3f, %.3f, %.3f, %.2f, %d, %d ;\r\n", X, Y, Z, A, InteriorSpawn, VirtualWorld);
			}
			else
			{
				format(sstring, sizeof(sstring), "%.3f, %.3f, %.3f, %.2f, %d, %d ; // %s\r\n", X, Y, Z, A, InteriorSpawn, VirtualWorld, comment);
			}
			hFile = fopen(SpawnPosFile, io_append);
			fwrite(hFile, sstring);
			fclose(hFile);

			AddSpawnPos(X, Y, Z, A, InteriorSpawn, VirtualWorld);
			SendClientMessage(playerid, Branco, "Posição para Spawn salva com sucesso!");
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp(cmd, "/autosalvar", true) == 0)
	{
		if(pAdmin[playerid] == 5)
		{
			if(autosave[playerid] == 0)
			{
				autosave[playerid] = 1;
				autotimer[playerid] = SetTimerEx("SalvePlayerPos", 10000, true, "e", playerid);

				SendClientMessage(playerid, COLOR_WHITE, "Auto salvar ligado com sucesso!");
				SendClientMessage(playerid, Amarelo, "Digite novamente /autosalvar para desligar.");
			}
			else if(autosave[playerid] == 1)
			{
				autosave[playerid] = 0;
				KillTimer(autotimer[playerid]);

				SendClientMessage(playerid, COLOR_WHITE, "Auto salvar desligado com sucesso!");
				SendClientMessage(playerid, Amarelo, "Digite novamente /autosalvar para ligar.");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/salvarpos", true) == 0)
	{
		if(pAdmin[playerid] == 5)
		{
			new File:hFile, sstring[256], V, M,
				Float:X, Float:Y, Float:Z, Float:A,
				INT, comment[64];

			if(fexist("SavedPositions.txt"))
			{
				hFile = fopen("SavedPositions.txt", io_append);
			}
			else
			{
				hFile = fopen("SavedPositions.txt", io_write);
			}
			if(IsPlayerInAnyVehicle(playerid))
			{
				V = GetPlayerVehicleID(playerid);

				M = GetVehicleModel(V);
				GetVehiclePos(V, X, Y, Z);
				GetVehicleZAngle(V, A);
			}
			else
			{
				M = GetPlayerSkin(playerid);
				GetPlayerPos(playerid, X, Y, Z);
				GetPlayerFacingAngle(playerid, A);
			}
			INT = GetPlayerInterior(playerid);

			if(sscanf(cmdtext, "s[11]dffffds[64]", cmd, M, X, Y, Z, A, INT, comment))
			{
				format(sstring, sizeof sstring, "%d, %.4f, %.4f, %.4f, %.4f, %d\r\n", M, X, Y, Z, A, INT);
			}
			else
			{
				format(sstring, sizeof sstring, "%d, %.4f, %.4f, %.4f, %.4f, %d // %s\r\n", M, X, Y, Z, A, INT, comment);
			}
			fwrite(hFile, sstring);
			fclose(hFile);

			SendClientMessage(playerid, COLOR_WHITE, "Posição salva com sucesso!");
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp(cmd, "/salvar", true) == 0)
	{
		new Float:X, Float:Y, Float:Z, INT;

		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(IsPlayerInAnyVehicle(playerid))
		{
			GetVehiclePos(GetPlayerVehicleID(playerid), X, Y, Z);
		}
		else
		{
			GetPlayerPos(playerid, X, Y, Z);
		}
		INT = GetPlayerInterior(playerid);

		dini_IntSet(file, "Continuar", 1);
		dini_FloatSet(file, "ContinuarX", X);
		dini_FloatSet(file, "ContinuarY", Y);
		dini_FloatSet(file, "ContinuarZ", Z);
		dini_IntSet(file, "ContinuarI", INT);

		SendClientMessage(playerid, COLOR_WHITE, "Posição salva com sucesso!");
		SendClientMessage(playerid, COLOR_GREEN, "Use /continuar para ir até a posição salva.");
		return 1;
	}

	if(strcmp(cmd, "/vendertodascasas", true) == 0)
	{
		if(PlayerInfo[playerid][SCON] == true)
		{
			new pickupid, iconid;

			for(new c = 0; c < MAX_CASAS; c++)
			{
				format(string, sizeof(string), PASTA_CASAS, c);
				if(dini_Exists(string))
				{
					if(dini_Int(string, "TDono") == 1)
					{
						format(file, sizeof(file), PASTA_CONTAS, dini_Get(string, "Dono"));
						if(dini_Exists(file))
						{
							dini_IntSet(file, "SaldoBancario", dini_Int(file, "SaldoBancario")+dini_Int(string, "Preco"));
							dini_FloatSet(file, "CasaX", Float:1410.5046);
							dini_FloatSet(file, "CasaY", Float:-1789.7197);
							dini_FloatSet(file, "CasaZ", Float:13.8285);
							dini_IntSet(file, "Casa", 0);
						}
						dini_IntSet(string, "TDono", 0);
						dini_Set(string, "Dono", "Ninguem");

						DestroyDynamicPickup(dini_Int(string, "Id"));

						pickupid = CreateDynamicPickup(1273, 1, dini_Float(string, "PosX"), dini_Float(string, "PosY"), dini_Float(string, "PosZ"), -1, -1, -1, 200.0);
						dini_IntSet(string, "Id", pickupid);

						DestroyDynamicMapIcon(dini_Int(string, "IconId"));

						iconid = CreateDynamicMapIcon(dini_Float(string, "PosX"), dini_Float(string, "PosY"), dini_Float(string, "PosZ"), 31, 0, -1, -1, -1, 100.0);
						dini_IntSet(string, "IconId", iconid);

						format(STRX, sizeof(STRX), "{00FF00}Casa ID: {FF0000}%d\n{00FF00}Dono: {FF0000}%s\n{00FF00}Morador: {FF0000}%s\n{00FF00}Valor: {FF0000}$%d\n\n{00FF00}Último uso: {FF0000}%s", c, dini_Get(string, "Dono"), dini_Get(string, "Morador"), dini_Int(string, "Preco"), dini_Get(string, "DataSet"));
						UpdateDynamic3DTextLabelText(ctextoid[c], -1, STRX);
					}
				}
			}
			SendClientMessage(playerid, Amarelo, "Todas casas foram vendidas.");
			SendClientMessage(playerid, Verde, "O dinheiro foi depositado na conta do dono.");
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp(cmd, "/vendertodasprops", true) == 0)
	{
		if(PlayerInfo[playerid][SCON] == true)
		{
			new pickupid, iconid;

			for(new p = 0; p < MAX_PROPS; p++)
			{
				format(string, sizeof(string), PASTA_PROPS, p);
				if(dini_Exists(string))
				{
					if(dini_Int(string, "TDono") == 1)
					{
						format(file, sizeof(file), PASTA_CONTAS, dini_Get(string, "Dono"));
						if(dini_Exists(file))
						{
							dini_IntSet(file, "SaldoBancario", dini_Int(file, "SaldoBancario")+dini_Int(string, "Preco"));
							dini_IntSet(file, "Prop", 0);
						}
						dini_IntSet(string, "TDono", 0);
						dini_Set(string, "Dono", "Ninguem");

						DestroyDynamicPickup(dini_Int(string, "Id"));

						pickupid = CreateDynamicPickup(1274, 1, dini_Float(string, "PosX"), dini_Float(string, "PosY"), dini_Float(string, "PosZ"), -1, -1, -1, 200.0);
						dini_IntSet(string, "Id", pickupid);

						DestroyDynamicMapIcon(dini_Int(string, "IconId"));

						iconid = CreateDynamicMapIcon(dini_Float(string, "PosX"), dini_Float(string, "PosY"), dini_Float(string, "PosZ"), 11, 0, -1, -1, -1, 100.0);
						dini_IntSet(string, "IconId", iconid);

						format(STRX, sizeof(STRX), "{FF0000}%s\n\n{00FF00}Prop ID: {FF0000}%d\n{00FF00}Dono: {FF0000}%s\n{00FF00}Valor: {FF0000}$%d\n\n{00FF00}Último uso: {FF0000}%s", dini_Get(string, "Nome"), p, dini_Get(string, "Dono"), dini_Int(string, "Preco"), dini_Get(string, "DataSet"));
						UpdateDynamic3DTextLabelText(ptextoid[p], -1, STRX);
					}
				}
			}
			SendClientMessage(playerid, Amarelo, "Todas propriedades foram vendidas.");
			SendClientMessage(playerid, Verde, "O dinheiro foi depositado na conta do dono.");
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp(cmd, "/vendertodosveiculos", true) == 0)
	{
		if(PlayerInfo[playerid][SCON] == true)
		{
			for(new carro = 0; carro < MAX_CONCES; carro++)
			{
				format(string, sizeof(string), PASTA_CONCE, carro);
				if(dini_Exists(string))
				{
					if(dini_Int(string, "TDono") == 1)
					{
						format(file, sizeof(file), PASTA_CONTAS, dini_Get(string, "Dono"));
						if(dini_Exists(file))
						{
							dini_IntSet(file, "SaldoBancario", dini_Int(file, "SaldoBancario")+dini_Int(string, "Preco"));
						}
						dini_IntSet(string, "TDono", 0);
						dini_Set(string, "Dono", "Ninguem");
						dini_Set(string, "Convidado1", "Ninguem");
						dini_Set(string, "Convidado2", "Ninguem");
						dini_Set(string, "Convidado3", "Ninguem");
						dini_IntSet(string, "CarVIP", 0);

						DestroyVehicle(dini_Int(string, "Id"));
						CriarVeiculo3(carro, dini_Int(string, "Modelo"), dini_Float(string, "CordX"), dini_Float(string, "CordY"), dini_Float(string, "CordZ"), dini_Float(string, "Angulo"), dini_Int(string, "Cor1"), dini_Int(string, "Cor2"));
					}
				}
			}
			SendClientMessage(playerid, Amarelo, "Todos veículos foram vendidos.");
			SendClientMessage(playerid, Verde, "O dinheiro foi depositado na conta do dono.");
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp(cmd, "/liberartodosportoes", true) == 0)
	{
		if(PlayerInfo[playerid][SCON] == true)
		{
			for(new portao = 0; portao < MAX_PORTOES; portao++)
			{
				format(string, sizeof(string), PASTA_PORTOES, portao);
				if(dini_Exists(string))
				{
					if(dini_Int(string, "TDono") == 1)
					{
						dini_Set(string, "Dono", "Ninguem");
						dini_Set(string, "Convidado1", "Ninguem");
						dini_Set(string, "Convidado2", "Ninguem");
						dini_Set(string, "Convidado3", "Ninguem");
						dini_IntSet(string, "TDono", 0);
					}
				}
			}
			SendClientMessage(playerid, Amarelo, "Todos portões foram liberados.");
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp(cmd, "/comprarprop", true) == 0)
	{
		new pickupid, iconid;

		for(new p = 0; p < MAX_PROPS; p++)
		{
			format(string, sizeof(string), PASTA_PROPS, p);
			if(dini_Exists(string))
			{
				if(IsPlayerInRangeOfPoint(playerid, 2.0, dini_Float(string, "PosX"), dini_Float(string, "PosY"), dini_Float(string, "PosZ")))
				{
					if(dini_Int(string, "TDono") == 0)
					{
						if(GetPlayerGrana(playerid) >= dini_Int(string, "Preco"))
						{
							if(GetProps(playerid) < 1)
							{
								dini_IntSet(string, "TDono", 1);
								dini_Set(string, "Dono", GetPlayerNameEx(playerid));
								GivePlayerGrana(playerid, -dini_Int(string, "Preco"));

								DestroyDynamicPickup(dini_Int(string, "Id"));

								pickupid = CreateDynamicPickup(1274, 1, dini_Float(string, "PosX"), dini_Float(string, "PosY"), dini_Float(string, "PosZ"), -1, -1, -1, 200.0);
								dini_IntSet(string, "Id", pickupid);

								DestroyDynamicMapIcon(dini_Int(string, "IconId"));

								iconid = CreateDynamicMapIcon(dini_Float(string, "PosX"), dini_Float(string, "PosY"), dini_Float(string, "PosZ"), 35, 0, -1, -1, -1, 100.0);
								dini_IntSet(string, "IconId", iconid);

								format(STRX, sizeof(STRX), "{FF0000}%s\n\n{00FF00}Prop ID: {FF0000}%d\n{00FF00}Dono: {FF0000}%s\n{00FF00}Valor: {FF0000}$%d\n\n{00FF00}Último uso: {FF0000}%s", dini_Get(string, "Nome"), p, GetPlayerNameEx(playerid), dini_Int(string, "Preco"), dini_Get(string, "DataSet"));
								UpdateDynamic3DTextLabelText(ptextoid[p], -1, STRX);

								OnPlayerCommandText(playerid, "/salvarprop");
								return 1;
							}
							else
							{
								SendClientMessage(playerid, Vermelho, "Você já possui uma propriedade!");
								return 1;
							}
						}
						else
						{
							SendClientMessage(playerid, Vermelho, "Você não tem dinheiro suficiente!");
							return 1;
						}
					}
					else
					{
						SendClientMessage(playerid, Vermelho, "Essa propriedade não está a venda!");
						return 1;
					}
				}
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/venderprop", true) == 0)
	{
		new pickupid, iconid;

		for(new p = 0; p < MAX_PROPS; p++)
		{
			format(string, sizeof(string), PASTA_PROPS, p);
			if(dini_Exists(string))
			{
				if(IsPlayerInRangeOfPoint(playerid, 2.0, dini_Float(string, "PosX"), dini_Float(string, "PosY"), dini_Float(string, "PosZ")))
				{
					if(dini_Int(string, "TDono") == 1)
					{
						if(strcmp(dini_Get(string, "Dono"), GetPlayerNameEx(playerid), false) == 0 || pAdmin[playerid] == 5 || PlayerInfo[playerid][SCON] == true)
						{
							format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
							dini_IntSet(file, "Prop", 0);

							dini_IntSet(string, "TDono", 0);
							dini_Set(string, "Dono", "Ninguem");

							GivePlayerGrana(playerid, dini_Int(string, "Preco"));
							DestroyDynamicPickup(dini_Int(string, "Id"));

							pickupid = CreateDynamicPickup(1274, 1, dini_Float(string, "PosX"), dini_Float(string, "PosY"), dini_Float(string, "PosZ"), -1, -1, -1, 200.0);
							dini_IntSet(string, "Id", pickupid);

							DestroyDynamicMapIcon(dini_Int(string, "IconId"));

							iconid = CreateDynamicMapIcon(dini_Float(string, "PosX"), dini_Float(string, "PosY"), dini_Float(string, "PosZ"), 11, 0, -1, -1, -1, 100.0);
							dini_IntSet(string, "IconId", iconid);

							format(STRX, sizeof(STRX), "{FF0000}%s\n\n{00FF00}Prop ID: {FF0000}%d\n{00FF00}Dono: {FF0000}Ninguem\n{00FF00}Valor: {FF0000}$%d\n\n{00FF00}Último uso: {FF0000}%s", dini_Get(string, "Nome"), p, dini_Int(string, "Preco"), dini_Get(string, "DataSet"));
							UpdateDynamic3DTextLabelText(ptextoid[p], -1, STRX);
							return 1;
						}
						else
						{
							SendClientMessage(playerid, Vermelho, "Esta propriedade não é sua!");
							return 1;
						}
					}
					else
					{
						SendClientMessage(playerid, Vermelho, "Esta propriedade já está a venda!");
						return 1;
					}
				}
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/sacarprop", true) == 0)
	{
		new strx[256];

		for(new p = 0; p < MAX_PROPS; p++)
		{
			format(string, sizeof(string), PASTA_PROPS, p);
			if(dini_Exists(string))
			{
				if(IsPlayerInRangeOfPoint(playerid, 2.0, dini_Float(string, "PosX"), dini_Float(string, "PosY"), dini_Float(string, "PosZ")))
				{
					if(dini_Int(string, "TDono") == 1)
					{
						if(strcmp(dini_Get(string, "Dono"), GetPlayerNameEx(playerid), false) == 0)
						{
							GivePlayerGrana(playerid, dini_Int(string, "Grana"));
							dini_IntSet(string, "Grana", 0);

							format(strx, sizeof(strx), "Você pegou $%d de sua propriedade!", dini_Int(string, "Grana"));
							SendClientMessage(playerid, Amarelo, strx);
							return 1;
						}
						else
						{
							SendClientMessage(playerid, Vermelho, "Esta propriedade não é sua!");
							return 1;
						}
					}
					else
					{
						SendClientMessage(playerid, Vermelho, "Esta propriedade não tem dono!");
						return 1;
					}
				}
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/propnm", true) == 0)
	{
		for(new p = 0; p < MAX_PROPS; p++)
		{
			format(string, sizeof(string), PASTA_PROPS, p);
			if(IsPlayerInRangeOfPoint(playerid, 2.0, dini_Float(string, "PosX"), dini_Float(string, "PosY"), dini_Float(string, "PosZ")))
			{
				if(dini_Int(string, "TDono") == 1 || pAdmin[playerid] == 5 || PlayerInfo[playerid][SCON] == true)
				{
					if(strcmp(dini_Get(string, "Dono"), GetPlayerNameEx(playerid), false) == 0 || pAdmin[playerid] == 5 || PlayerInfo[playerid][SCON] == true)
					{
						new nome[64];

						if(sscanf(cmdtext, "s[8]s[64]", cmd, nome))
						{
							SendClientMessage(playerid, COLOR_RED, "/propnm [nome]");
							return 1;
						}
						else
						{
							dini_Set(string, "Nome", nome);

							format(STRX, sizeof(STRX), "{FF0000}%s\n\n{00FF00}Prop ID: {FF0000}%d\n{00FF00}Dono: {FF0000}%s\n{00FF00}Valor: {FF0000}$%d\n\n{00FF00}Último uso: {FF0000}%s", nome, p, dini_Get(string, "Dono"), dini_Int(string, "Preco"), dini_Get(string, "DataSet"));
							UpdateDynamic3DTextLabelText(ptextoid[p], -1, STRX);
							return 1;
						}
					}
					else
					{
						SendClientMessage(playerid, Vermelho, "Esta propriedade não é sua!");
						return 1;
					}
				}
				else
				{
					SendClientMessage(playerid, Vermelho, "Esta propriedade não tem dono!");
					return 1;
				}
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/criarprop", true) == 0)
	{
		if(pAdmin[playerid] == 5)
		{
			new preco, Float:X, Float:Y, Float:Z;

			if(sscanf(cmdtext, "s[11]d", cmd, preco))
			{
				SendClientMessage(playerid, Vermelho, "/criarprop [preço]");
				return 1;
			}
			GetPlayerPos(playerid, X, Y, Z);
			PlayerCreateProp(playerid, preco, X, Y, Z, GetPlayerInterior(playerid));
		}
		return 1;
	}

	if(strcmp(cmd, "/mudarprop", true) == 0)
	{
		new id, pickupid, iconid, string222[256],
			Float:X, Float:Y, Float:Z, I;

		if(sscanf(cmdtext, "s[11]d", cmd, id))
		{
			SendClientMessage(playerid, Vermelho, "/mudarprop [id]");
			return 1;
		}
		if(pAdmin[playerid] > 3)
		{
			format(string, sizeof(string), PASTA_PROPS, id);
			if(dini_Exists(string))
			{
				GetPlayerPos(playerid, X, Y, Z);
				I = GetPlayerInterior(playerid);

				dini_FloatSet(string, "PosX", X);
				dini_FloatSet(string, "PosY", Y);
				dini_FloatSet(string, "PosZ", Z);
				dini_IntSet(string, "IntID", I);

				DestroyDynamicPickup(dini_Int(string, "Id"));
				DestroyDynamicMapIcon(dini_Int(string, "IconId"));
				DestroyDynamic3DTextLabel(ptextoid[id]);
				ptextoid[id] = Text3D:INVALID_3DTEXT_ID;

				format(file, sizeof(file), PASTA_CONTAS, dini_Get(string, "Dono"));
				if(dini_Exists(file))
				{
					dini_FloatSet(file, "PropX", Float:X);
					dini_FloatSet(file, "PropY", Float:Y);
					dini_FloatSet(file, "PropZ", Float:Z);

					format(string222, sizeof(string222), "Dono Spawned: %s", dini_Get(string, "Dono"));
					SendClientMessage(playerid, BLUEWHITE, string222);
				}
				pickupid = CreateDynamicPickup(1274, 1, X, Y, Z, -1, -1, -1, 200.0);
				dini_IntSet(string, "Id", pickupid);

				iconid = CreateDynamicMapIcon(X, Y, Z, 11, 0, -1, -1, -1, 100.0);
				dini_IntSet(string, "IconId", iconid);

				format(STRX, sizeof(STRX), "{FF0000}%s\n\n{00FF00}Prop ID: {FF0000}%d\n{00FF00}Dono: {FF0000}%s\n{00FF00}Valor: {FF0000}$%d\n\n{00FF00}Último uso: {FF0000}%s", dini_Get(string, "Nome"), id, dini_Get(string, "Dono"), dini_Int(string, "Preco"), dini_Get(string, "DataSet"));
				ptextoid[id] = CreateDynamic3DTextLabel(STRX, -1, X, Y, Z, 30.0, INVALID_PLAYER_ID, INVALID_VEHICLE_ID, 1, -1, -1, -1, 200.0);
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente!");
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp(cmd, "/criarcasa", true) == 0)
	{
		if(pAdmin[playerid] == 5)
		{
			new preco, cint,
				Float:X, Float:Y, Float:Z;

			if(sscanf(cmdtext, "s[11]dd", cmd, preco, cint))
			{
				SendClientMessage(playerid, Vermelho, "/criarcasa [preço] [interior]");
				return 1;
			}
			GetPlayerPos(playerid, X, Y, Z);
			PlayerCreateHause(playerid, preco, cint, X, Y, Z, GetPlayerInterior(playerid));
		}
		return 1;
	}

	if(strcmp(cmd, "/mudarcasa", true) == 0)
	{
		new id, pickupid, iconid, string222[256],
			Float:X, Float:Y, Float:Z, I;

		if(sscanf(cmdtext, "s[11]d", cmd, id))
		{
			SendClientMessage(playerid, Vermelho, "/mudarcasa [id]");
			return 1;
		}
		if(pAdmin[playerid] > 3)
		{
			format(string, sizeof(string), PASTA_CASAS, id);
			if(dini_Exists(string))
			{
				GetPlayerPos(playerid, X, Y, Z);
				I = GetPlayerInterior(playerid);

				dini_FloatSet(string, "PosX", Float:X);
				dini_FloatSet(string, "PosY", Float:Y);
				dini_FloatSet(string, "PosZ", Float:Z);
				dini_IntSet(string, "IntID", I);

				DestroyDynamicPickup(dini_Int(string, "Id"));
				DestroyDynamicMapIcon(dini_Int(string, "IconId"));
				DestroyDynamic3DTextLabel(ctextoid[id]);
				ctextoid[id] = Text3D:INVALID_3DTEXT_ID;

				format(file, sizeof(file), PASTA_CONTAS, dini_Get(string, "Dono"));
				if(dini_Exists(file))
				{
					dini_FloatSet(file, "CasaX", Float:X);
					dini_FloatSet(file, "CasaY", Float:Y);
					dini_FloatSet(file, "CasaZ", Float:Z);

					format(string222, sizeof(string222), "Dono Spawned: %s", dini_Get(string, "Dono"));
					SendClientMessage(playerid, BLUEWHITE, string222);
				}
				pickupid = CreateDynamicPickup(1273, 1, X, Y, Z, -1, -1, -1, 200.0);
				dini_IntSet(string, "Id", pickupid);

				iconid = CreateDynamicMapIcon(X, Y, Z, 31, 0, -1, -1, -1, 100.0);
				dini_IntSet(string, "IconId", iconid);

				format(STRX, sizeof(STRX), "{00FF00}Casa ID: {FF0000}%d\n{00FF00}Dono: {FF0000}%s\n{00FF00}Morador: {FF0000}%s\n{00FF00}Valor: {FF0000}$%d\n\n{00FF00}Último uso: {FF0000}%s", id, dini_Get(string, "Dono"), dini_Get(string, "Morador"), dini_Int(string, "Preco"), dini_Get(string, "DataSet"));
				ctextoid[id] = CreateDynamic3DTextLabel(STRX, -1, X, Y, Z, 30.0, INVALID_PLAYER_ID, INVALID_VEHICLE_ID, 1, -1, -1, -1, 200.0);
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente!");
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp(cmd, "/precoprop", true) == 0)
	{
		new preco;

		if(sscanf(cmdtext, "s[11]d", cmd, preco))
		{
			SendClientMessage(playerid, Vermelho, "/precoprop [preço]");
			return 1;
		}
		for(new p = 0; p < MAX_PROPS; p++)
		{
			format(string, sizeof(string), PASTA_PROPS, p);
			if(dini_Exists(string))
			{
				if(IsPlayerInRangeOfPoint(playerid, 2.0, dini_Float(string, "PosX"), dini_Float(string, "PosY"), dini_Float(string, "PosZ")))
				{
					if(pAdmin[playerid] >= 5)
					{
						dini_IntSet(string, "Preco", preco);

						format(STRX, sizeof(STRX), "{FF0000}%s\n\n{00FF00}Prop ID: {FF0000}%d\n{00FF00}Dono: {FF0000}%s\n{00FF00}Valor: {FF0000}$%d\n\n{00FF00}Último uso: {FF0000}%s", dini_Get(string, "Nome"), p, dini_Get(string, "Dono"), preco, dini_Get(string, "DataSet"));
						UpdateDynamic3DTextLabelText(ptextoid[p], -1, STRX);
						return 1;
					}
					else
					{
						SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
						return 1;
					}
				}
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/comprarcasa", true) == 0)
	{
		new pickupid, iconid;

		for(new c = 0; c < MAX_CASAS; c++)
		{
			format(string, sizeof(string), PASTA_CASAS, c);
			if(dini_Exists(string))
			{
				if(IsPlayerInRangeOfPoint(playerid, 2.0, dini_Float(string, "PosX"), dini_Float(string, "PosY"), dini_Float(string, "PosZ")))
				{
					if(dini_Int(string, "TDono") == 0)
					{
						if(GetPlayerGrana(playerid) >= dini_Int(string, "Preco"))
						{
							if(GetCasas(playerid) < 1)
							{
								dini_IntSet(string, "TDono", 1);
								dini_Set(string, "Dono", GetPlayerNameEx(playerid));
								GivePlayerGrana(playerid, -dini_Int(string, "Preco"));

								DestroyDynamicPickup(dini_Int(string, "Id"));

								pickupid = CreateDynamicPickup(1272, 1, dini_Float(string, "PosX"), dini_Float(string, "PosY"), dini_Float(string, "PosZ"), -1, -1, -1, 200.0);
								dini_IntSet(string, "Id", pickupid);

								DestroyDynamicMapIcon(dini_Int(string, "IconId"));

								iconid = CreateDynamicMapIcon(dini_Float(string, "PosX"), dini_Float(string, "PosY"), dini_Float(string, "PosZ"), 32, 0, -1, -1, -1, 100.0);
								dini_IntSet(string, "IconId", iconid);

								format(STRX, sizeof(STRX), "{00FF00}Casa ID: {FF0000}%d\n{00FF00}Dono: {FF0000}%s\n{00FF00}Morador: {FF0000}%s\n{00FF00}Valor: {FF0000}$%d\n\n{00FF00}Último uso: {FF0000}%s", c, GetPlayerNameEx(playerid), dini_Get(string, "Morador"), dini_Int(string, "Preco"), dini_Get(string, "DataSet"));
								UpdateDynamic3DTextLabelText(ctextoid[c], -1, STRX);

								OnPlayerCommandText(playerid, "/nascercasa");
								return 1;
							}
							else
							{
								SendClientMessage(playerid, Vermelho, "Você só pode ter uma casa!");
								return 1;
							}
						}
						else
						{
							SendClientMessage(playerid, Vermelho, "Você não tem dinheiro!");
							return 1;
						}
					}
					else
					{
						SendClientMessage(playerid, Vermelho, "Esta casa não está a venda!");
						return 1;
					}
				}
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/precocasa", true) == 0)
	{
		new preco;

		if(sscanf(cmdtext, "s[11]d", cmd, preco))
		{
			SendClientMessage(playerid, Vermelho, "/precocasa [preço]");
			return 1;
		}
		for(new c = 0; c < MAX_CASAS; c++)
		{
			format(string, sizeof(string), PASTA_CASAS, c);
			if(dini_Exists(string))
			{
				if(IsPlayerInRangeOfPoint(playerid, 2.0, dini_Float(string, "PosX"), dini_Float(string, "PosY"), dini_Float(string, "PosZ")))
				{
					if(pAdmin[playerid] > 4)
					{
						dini_IntSet(string, "Preco", preco);

						format(STRX, sizeof(STRX), "{00FF00}Casa ID: {FF0000}%d\n{00FF00}Dono: {FF0000}%s\n{00FF00}Morador: {FF0000}%s\n{00FF00}Valor: {FF0000}$%d\n\n{00FF00}Último uso: {FF0000}%s", c, dini_Get(string, "Dono"), dini_Get(string, "Morador"), preco, dini_Get(string, "DataSet"));
						UpdateDynamic3DTextLabelText(ctextoid[c], -1, STRX);
						return 1;
					}
					else
					{
						SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
						return 1;
					}
				}
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/precocarro", true) == 0)
	{
		new preco;

		if(sscanf(cmdtext, "s[12]d", cmd, preco))
		{
			SendClientMessage(playerid, Vermelho, "/precocarro [preço]");
			return 1;
		}
		for(new c = 0; c < MAX_CONCES; c++)
		{
			format(string, sizeof(string), PASTA_CONCE, c);
			if(dini_Exists(string))
			{
				if(GetPlayerVehicleID(playerid) == dini_Int(string, "Id"))
				{
					if(pAdmin[playerid] > 4)
					{
						dini_IntSet(string, "Preco", preco);
						return 1;
					}
					else
					{
						SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
						return 1;
					}
				}
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/convidarcasa", true) == 0)
	{
		new pid;

		if(sscanf(cmdtext, "s[14]u", cmd, pid))
		{
			SendClientMessage(playerid, Vermelho, "/convidarcasa [id]");
			return 1;
		}
		if(IsPlayerConnected(pid))
		{
			for(new c = 0; c < MAX_CASAS; c++)
			{
				format(string, sizeof(string), PASTA_CASAS, c);
				if(IsPlayerInRangeOfPoint(playerid, 2.0, dini_Float(string, "PosX"), dini_Float(string, "PosY"), dini_Float(string, "PosZ")))
				{
					if(dini_Exists(string))
					{
						if(strcmp(dini_Get(string, "Dono"), GetPlayerNameEx(playerid), false) == 0)
						{
							if(dini_Int(string, "TMorador") == 1)
							{
								SendClientMessage(playerid, Vermelho, "Já tem um morador em sua casa.");
								return 1;
							}
							morar[pid] = 1;
							moradia[pid] = c;

							convitede[pid] = playerid;
							MoradiaTime[pid] = SetTimerEx("Moradia", 30000, false, "d", pid);

							format(STRX, sizeof(STRX), "%s (ID: %d) está convidando você para morar na casa dele(a).", GetPlayerNameEx(playerid), playerid);
							SendClientMessage(pid, Amarelo, STRX);

							SendClientMessage(pid, Amarelo, "Para aceitar o convite, use: /aceitarmoradia || Para recusar, use: /recusarmoradia");
							SendClientMessage(playerid, Vermelho, "Convite enviado.");
							return 1;
						}
						else
						{
							SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
							return 1;
						}
					}
				}
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente!");
		}
		return 1;
	}

	if(strcmp(cmd, "/expulsarcasa", true) == 0)
	{
		new string222[256], nick[MAX_PLAYER_NAME];

		if(sscanf(cmdtext, "s[14]s[24]", cmd, nick))
		{
			SendClientMessage(playerid, Vermelho, "/expulsarcasa [nick]");
			return 1;
		}
		for(new c = 0; c < MAX_CASAS; c++)
		{
			format(string, sizeof(string), PASTA_CASAS, c);
			if(dini_Exists(string))
			{
				if(IsPlayerInRangeOfPoint(playerid, 2.0, dini_Float(string, "PosX"), dini_Float(string, "PosY"), dini_Float(string, "PosZ")))
				{
					if(strcmp(dini_Get(string, "Dono"), GetPlayerNameEx(playerid), false) == 0)
					{
						if(strcmp(dini_Get(string, "Morador"), nick, false) == 0 && dini_Int(string, "TMorador") == 1)
						{
							format(file, sizeof(file), PASTA_CONTAS, dini_Get(string, "Morador"));
							if(dini_Exists(file))
							{
								dini_FloatSet(file, "CasaX", Float:1410.5046);
								dini_FloatSet(file, "CasaY", Float:-1789.7197);
								dini_FloatSet(file, "CasaZ", Float:13.8285);

								format(string222, sizeof(string222), "Morador Spawned: %s", dini_Get(string, "Morador"));
								SendClientMessage(playerid, BLUEWHITE, string222);
							}
							dini_IntSet(string, "TMorador", 0);
							dini_Set(string, "Morador", "Ninguem");

							format(STRX, sizeof(STRX), "{00FF00}Casa ID: {FF0000}%d\n{00FF00}Dono: {FF0000}%s\n{00FF00}Morador: {FF0000}Ninguem\n{00FF00}Valor: {FF0000}$%d\n\n{00FF00}Último uso: {FF0000}%s", c, dini_Get(string, "Dono"), dini_Int(string, "Preco"), dini_Get(string, "DataSet"));
							UpdateDynamic3DTextLabelText(ctextoid[c], -1, STRX);

							SendClientMessage(playerid, Verde, "Expulso!");
							return 1;
						}
						else
						{
							SendClientMessage(playerid, Vermelho, "Esse jogador não mora aqui!");
							return 1;
						}
					}
					else
					{
						SendClientMessage(playerid, Vermelho, "Sua casa não tem morador.");
						return 1;
					}
				}
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/deletarcasa", true) == 0)
	{
		new casaid;

		if(sscanf(cmdtext, "s[13]d", cmd, casaid))
		{
			SendClientMessage(playerid, Vermelho, "/deletarcasa [casaid]");
			return 1;
		}
		format(string, sizeof(string), PASTA_CASAS, casaid);
		if(dini_Exists(string))
		{
			if(pAdmin[playerid] > 4)
			{
				DestroyDynamicPickup(dini_Int(string, "Id"));
				DestroyDynamicMapIcon(dini_Int(string, "IconId"));
				DestroyDynamic3DTextLabel(ctextoid[casaid]);
				ctextoid[casaid] = Text3D:INVALID_3DTEXT_ID;

				dini_IntSet(string, "TDono", 3);
				dini_Set(string, "Dono", "Ninguem");
				dini_IntSet(string, "Id", -1);
				dini_IntSet(string, "IconId", -1);

				SendClientMessage(playerid, roxo, "Casa deletada com sucesso!");
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/deletarprop", true) == 0)
	{
		new propid;

		if(sscanf(cmdtext, "s[13]d", cmd, propid))
		{
			SendClientMessage(playerid, Vermelho, "/deletarprop [propid]");
			return 1;
		}
		format(string, sizeof(string), PASTA_PROPS, propid);
		if(dini_Exists(string))
		{
			if(pAdmin[playerid] >= 5)
			{
				DestroyDynamicPickup(dini_Int(string, "Id"));
				DestroyDynamicMapIcon(dini_Int(string, "IconId"));
				DestroyDynamic3DTextLabel(ptextoid[propid]);
				ptextoid[propid] = Text3D:INVALID_3DTEXT_ID;

				dini_IntSet(string, "TDono", 3);
				dini_Set(string, "Dono", "Ninguem");
				dini_IntSet(string, "Id", -1);
				dini_IntSet(string, "IconId", -1);

				SendClientMessage(playerid, roxo, "Propriedade deletada com sucesso!");
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/liberarcarro", true) == 0)
	{
		new conceid, carroid;

		if(sscanf(cmdtext, "s[14]d", cmd, conceid))
		{
			SendClientMessage(playerid, Vermelho, "/liberarcarro [carro]");
			return 1;
		}
		format(string, sizeof(string), PASTA_CONCE, conceid);
		if(dini_Exists(string))
		{
			if(pAdmin[playerid] >= 5)
			{
				if(!(dini_Int(string, "TDono") == 3))
				{
					DestroyVehicle(dini_Int(string, "Id"));
				}
				dini_IntSet(string, "TDono", 0);
				dini_Set(string, "Dono", "Ninguem");
				dini_IntSet(string, "CarVIP", 0);

				dini_Set(string, "Convidado1", "Ninguem");
				dini_Set(string, "Convidado2", "Ninguem");
				dini_Set(string, "Convidado3", "Ninguem");

				carroid = AddStaticVehicle(dini_Int(string, "Modelo"), dini_Float(string, "CordX"), dini_Float(string, "CordY"), dini_Float(string, "CordZ"), dini_Float(string, "Angulo"), dini_Int(string, "Cor1"), dini_Int(string, "Cor2"));
				dini_IntSet(string, "Id", carroid);

				format(string, sizeof(string), "O(A) ADM %s liberou o veículo: %d", GetPlayerNameEx(playerid), conceid);
				SendClientMessageToAll(tcadm, string);
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/liberarcasa", true) == 0)
	{
		new casaid, pickupid, iconid;

		if(sscanf(cmdtext, "s[13]d", cmd, casaid))
		{
			SendClientMessage(playerid, Vermelho, "/liberarcasa [casa]");
			return 1;
		}
		format(string, sizeof(string), PASTA_CASAS, casaid);
		if(dini_Exists(string))
		{
			if(pAdmin[playerid] >= 5)
			{
				if(!(dini_Int(string, "TDono") == 3))
				{
					DestroyDynamicPickup(dini_Int(string, "Id"));
					DestroyDynamicMapIcon(dini_Int(string, "IconId"));
					DestroyDynamic3DTextLabel(ctextoid[casaid]);
					ctextoid[casaid] = Text3D:INVALID_3DTEXT_ID;
				}
				format(file, sizeof(file), PASTA_CONTAS, dini_Get(string, "Dono"));
				if(dini_Exists(file))
				{
					dini_FloatSet(file, "CasaX", Float:1410.5046);
					dini_FloatSet(file, "CasaY", Float:-1789.7197);
					dini_FloatSet(file, "CasaZ", Float:13.8285);
				}
				dini_IntSet(string, "TDono", 0);
				dini_Set(string, "Dono", "Ninguem");

				pickupid = CreateDynamicPickup(1273, 1, dini_Float(string, "PosX"), dini_Float(string, "PosY"), dini_Float(string, "PosZ"), -1, -1, -1, 200.0);
				dini_IntSet(string, "Id", pickupid);

				iconid = CreateDynamicMapIcon(dini_Float(string, "PosX"), dini_Float(string, "PosY"), dini_Float(string, "PosZ"), 31, 0, -1, -1, -1, 100.0);
				dini_IntSet(string, "IconId", iconid);

				format(STRX, sizeof(STRX), "{00FF00}Casa ID: {FF0000}%d\n{00FF00}Dono: {FF0000}%s\n{00FF00}Morador: {FF0000}%s\n{00FF00}Valor: {FF0000}$%d\n\n{00FF00}Último uso: {FF0000}%s", casaid, dini_Get(string, "Dono"), dini_Get(string, "Morador"), dini_Int(string, "Preco"), dini_Get(string, "DataSet"));
				ctextoid[casaid] = CreateDynamic3DTextLabel(STRX, -1, dini_Float(string, "PosX"), dini_Float(string, "PosY"), dini_Float(string, "PosZ"), 30.0, INVALID_PLAYER_ID, INVALID_VEHICLE_ID, 1, -1, -1, -1, 200.0);

				format(string, sizeof(string), "O(A) ADM %s liberou a casa: %d", GetPlayerNameEx(playerid), casaid);
				SendClientMessageToAll(tcadm, string);
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/liberarprop", true) == 0)
	{
		new propid, iconid;

		if(sscanf(cmdtext, "s[13]d", cmd, propid))
		{
			SendClientMessage(playerid, Vermelho, "/liberarprop [prop]");
			return 1;
		}
		format(string, sizeof(string), PASTA_PROPS, propid);
		if(dini_Exists(string))
		{
			if(pAdmin[playerid] >= 5)
			{
				if(!(dini_Int(string, "TDono") == 3))
				{
					DestroyDynamicMapIcon(dini_Int(string, "IconId"));
					DestroyDynamic3DTextLabel(ptextoid[propid]);
					ptextoid[propid] = Text3D:INVALID_3DTEXT_ID;
				}
				dini_IntSet(string, "TDono", 0);
				dini_Set(string, "Dono", "Ninguem");

				iconid = CreateDynamicMapIcon(dini_Float(string, "PosX"), dini_Float(string, "PosY"), dini_Float(string, "PosZ"), 11, 0, -1, -1, -1, 100.0);
				dini_IntSet(string, "IconId", iconid);

				format(STRX, sizeof(STRX), "{FF0000}%s\n\n{00FF00}Prop ID: {FF0000}%d\n{00FF00}Dono: {FF0000}%s\n{00FF00}Valor: {FF0000}$%d\n\n{00FF00}Último uso: {FF0000}%s", dini_Get(string, "Nome"), propid, dini_Get(string, "Dono"), dini_Int(string, "Preco"), dini_Get(string, "DataSet"));
				ptextoid[propid] = CreateDynamic3DTextLabel(STRX, -1, dini_Float(string, "PosX"), dini_Float(string, "PosY"), dini_Float(string, "PosZ"), 30.0, INVALID_PLAYER_ID, INVALID_VEHICLE_ID, 1, -1, -1, -1, 200.0);

				format(string, sizeof(string), "O(A) ADM %s liberou a propriedade: %d", GetPlayerNameEx(playerid), propid);
				SendClientMessageToAll(tcadm, string);
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/interiorcasa", true) == 0)
	{
		new intid;

		if(sscanf(cmdtext, "s[14]d", cmd, intid))
		{
			SendClientMessage(playerid, Vermelho, "/interiorcasa [interior]");
			return 1;
		}
		for(new c = 0; c < MAX_CASAS; c++)
		{
			format(string, sizeof(string), PASTA_CASAS, c);
			if(dini_Exists(string))
			{
				if(IsPlayerInRangeOfPoint(playerid, 2.0, dini_Float(string, "PosX"), dini_Float(string, "PosY"), dini_Float(string, "PosZ")))
				{
					if(pAdmin[playerid] >= 5)
					{
						dini_IntSet(string, "Int", intid);
						return 1;
					}
					else
					{
						SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
						return 1;
					}
				}
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/ircasa", true) == 0)
	{
		new id;

		if(sscanf(cmdtext, "s[8]d", cmd, id))
		{
			SendClientMessage(playerid, Vermelho, "/ircasa [id]");
			return 1;
		}
		format(string, sizeof(string), PASTA_CASAS, id);
		if(dini_Exists(string))
		{
			if(pAdmin[playerid] > 3)
			{
				SetPlayerInterior(playerid, dini_Int(string, "IntID"));
				SetPlayerPos(playerid, dini_Float(string, "PosX"), dini_Float(string, "PosY"), dini_Float(string, "PosZ"));

				TogglePlayerControllable(playerid, false);
				SetTimerEx("DestogglePlayerControllable", 1000, 0, "d", playerid);
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/irprop", true) == 0)
	{
		new id;

		if(sscanf(cmdtext, "s[8]d", cmd, id))
		{
			SendClientMessage(playerid, Vermelho, "/irprop [id]");
			return 1;
		}
		format(string, sizeof(string), PASTA_PROPS, id);
		if(dini_Exists(string))
		{
			if(pAdmin[playerid] > 4)
			{
				SetPlayerInterior(playerid, dini_Int(string, "IntID"));
				SetPlayerPos(playerid, dini_Float(string, "PosX"), dini_Float(string, "PosY"), dini_Float(string, "PosZ"));

				TogglePlayerControllable(playerid, false);
				SetTimerEx("DestogglePlayerControllable", 1000, 0, "d", playerid);
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/vendercasa", true) == 0)
	{
		new pickupid, iconid;

		for(new c = 0; c < MAX_CASAS; c++)
		{
			format(string, sizeof(string), PASTA_CASAS, c);
			if(dini_Exists(string))
			{
				if(IsPlayerInRangeOfPoint(playerid, 2.0, dini_Float(string, "PosX"), dini_Float(string, "PosY"), dini_Float(string, "PosZ")))
				{
					if(dini_Int(string, "TDono") == 1)
					{
						if(strcmp(dini_Get(string, "Dono"), GetPlayerNameEx(playerid), false) == 0 || pAdmin[playerid] == 5 || PlayerInfo[playerid][SCON] == true)
						{
							format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
							if(strcmp(dini_Get(string, "Dono"), GetPlayerNameEx(playerid), false) == 0)
							{
								dini_FloatSet(file, "CasaX", Float:1410.5046);
								dini_FloatSet(file, "CasaY", Float:-1789.7197);
								dini_FloatSet(file, "CasaZ", Float:13.8285);
							}
							dini_IntSet(file, "Casa", 0);
							dini_IntSet(string, "TDono", 0);
							dini_Set(string, "Dono", "Ninguem");
							GivePlayerGrana(playerid, dini_Int(string, "Preco"));

							DestroyDynamicPickup(dini_Int(string, "Id"));

							pickupid = CreateDynamicPickup(1273, 1, dini_Float(string, "PosX"), dini_Float(string, "PosY"), dini_Float(string, "PosZ"), -1, -1, -1, 200.0);
							dini_IntSet(string, "Id", pickupid);

							DestroyDynamicMapIcon(dini_Int(string, "IconId"));

							iconid = CreateDynamicMapIcon(dini_Float(string, "PosX"), dini_Float(string, "PosY"), dini_Float(string, "PosZ"), 31, 0, -1, -1, -1, 100.0);
							dini_IntSet(string, "IconId", iconid);

							format(STRX, sizeof(STRX), "{00FF00}Casa ID: {FF0000}%d\n{00FF00}Dono: {FF0000}Ninguem\n{00FF00}Morador: {FF0000}%s\n{00FF00}Valor: {FF0000}$%d\n\n{00FF00}Último uso: {FF0000}%s", c, dini_Get(string, "Morador"), dini_Int(string, "Preco"), dini_Get(string, "DataSet"));
							UpdateDynamic3DTextLabelText(ctextoid[c], -1, STRX);
							return 1;
						}
						else
						{
							SendClientMessage(playerid, Vermelho, "Esta casa não é sua.");
							return 1;
						}
					}
					else
					{
						SendClientMessage(playerid, Vermelho, "Esta casa já está a venda!");
						return 1;
					}
				}
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/desbugarcarro", true) == 0)
	{
		new id, Float:X, Float:Y, Float:Z;

		if(sscanf(cmdtext, "s[15]d", cmd, id))
		{
			SendClientMessage(playerid, Vermelho, "/desbugarcarro [carro]");
			return 1;
		}
		if(pAdmin[playerid] > 4)
		{
			format(string, sizeof(string), PASTA_CONCE, id);
			if(dini_Exists(string))
			{
				GetVehiclePos(dini_Int(string, "Id"), X, Y, Z);
				if(strcmp(dini_Get(string, "Dono"), "Ninguem", true) == 0)
				{
					if(!(dini_Int(string, "TDono") == 3))
					{
						dini_IntSet(string, "TDono", 0);
					}
				}
				DestroyVehicle(dini_Int(string, "Id"));
				CriarVeiculo3(id, dini_Int(string, "Modelo"), dini_Float(string, "CordX"), dini_Float(string, "CordY"), dini_Float(string, "CordZ"), dini_Float(string, "Angulo"), dini_Int(string, "Cor1"), dini_Int(string, "Cor2"));
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp(cmd, "/ircarro", true) == 0)
	{
		new id, Float:X, Float:Y, Float:Z;

		if(sscanf(cmdtext, "s[9]d", cmd, id))
		{
			SendClientMessage(playerid, Vermelho, "/ircarro [id]");
			return 1;
		}
		if(pAdmin[playerid] > 3)
		{
			format(string, sizeof(string), PASTA_CONCE, id);
			if(dini_Exists(string))
			{
				GetVehiclePos(dini_Int(string, "Id"), X, Y, Z);
				if(IsVehicleOccupied(dini_Int(string, "Id")))
				{
					SetPlayerPos(playerid, X, Y, Z+4);
				}
				else
				{
					PutPlayerInVehicle(playerid, dini_Int(string, "Id"), 0);
				}
				TogglePlayerControllable(playerid, false);
				SetTimerEx("DestogglePlayerControllable", 1000, 0, "d", playerid);
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp(cmd, "/nascercasa", true) == 0)
	{
		new Float:X, Float:Y, Float:Z;

		for(new c = 0; c < MAX_CASAS; c++)
		{
			format(string, sizeof(string), PASTA_CASAS, c);
			if(dini_Exists(string))
			{
				if(IsPlayerInRangeOfPoint(playerid, 2.0, dini_Float(string, "PosX"), dini_Float(string, "PosY"), dini_Float(string, "PosZ")))
				{
					if(dini_Int(string, "TDono") == 1)
					{
						if(strcmp(dini_Get(string, "Dono"), GetPlayerNameEx(playerid), false) == 0)
						{
							format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
							if(dini_Exists(file))
							{
								GetPlayerPos(playerid, X, Y, Z);
								dini_FloatSet(file, "CasaX", Float:X);
								dini_FloatSet(file, "CasaY", Float:Y);
								dini_FloatSet(file, "CasaZ", Float:Z);
								dini_IntSet(file, "Casa", 1);
							}
							SetSpawnInfo(playerid, 1, dini_Int(file, "Skin"), dini_Int(file, "CasaX"), dini_Int(file, "CasaY"), dini_Int(file, "CasaZ"), 354.1657, 0, 0, 0, 0, 0, 0);
							return 1;
						}
						else
						{
							SendClientMessage(playerid, Vermelho, "Esta casa não é sua.");
							return 1;
						}
					}
					else
					{
						SendClientMessage(playerid, Vermelho, "Esta casa não é sua.");
						return 1;
					}
				}
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/salvarprop", true) == 0)
	{
		new Float:X, Float:Y, Float:Z;

		for(new c = 0; c < MAX_PROPS; c++)
		{
			format(string, sizeof(string), PASTA_PROPS, c);
			if(dini_Exists(string))
			{
				if(IsPlayerInRangeOfPoint(playerid, 2.0, dini_Float(string, "PosX"), dini_Float(string, "PosY"), dini_Float(string, "PosZ")))
				{
					if(dini_Int(string, "TDono") == 1)
					{
						if(strcmp(dini_Get(string, "Dono"), GetPlayerNameEx(playerid), false) == 0)
						{
							format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
							if(dini_Exists(string))
							{
								GetPlayerPos(playerid, X, Y, Z);
								dini_FloatSet(file, "PropX", Float:X);
								dini_FloatSet(file, "PropY", Float:Y);
								dini_FloatSet(file, "PropZ", Float:Z);
								dini_IntSet(file, "Prop", 1);
								return 1;
							}
						}
						else
						{
							SendClientMessage(playerid, Vermelho, "Esta propriedade não é sua.");
							return 1;
						}
					}
					else
					{
						SendClientMessage(playerid, Vermelho, "Esta propriedade não é sua.");
						return 1;
					}
				}
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/aceitarmoradia", true) == 0)
	{
		for(new c = 0; c < MAX_CASAS; c++)
		{
			format(string, sizeof(string), PASTA_CASAS, c);
			if(dini_Int(string, "TMorador") == 1)
			{
				if(strcmp(dini_Get(string, "Morador"), GetPlayerNameEx(playerid), false) == 0)
				{
					dini_IntSet(string, "TMorador", 0);
					dini_Set(string, "Morador", "Ninguem");
				}
			}
		}
		format(string, sizeof(string), PASTA_CASAS, moradia[playerid]);
		if(dini_Exists(string))
		{
			if(morar[playerid] == 1)
			{
				format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));

				morar[playerid] = 0;
				moradia[playerid] = 0;

				KillTimer(MoradiaTime[playerid]);
				convitede[playerid] = INVALID_PLAYER_ID;

				dini_IntSet(string, "TMorador", 1);
				dini_Set(string, "Morador", GetPlayerNameEx(playerid));
				dini_FloatSet(file, "CasaX" , dini_Float(string, "PosX"));
				dini_FloatSet(file, "CasaY" , dini_Float(string, "PosY"));
				dini_FloatSet(file, "CasaZ" , dini_Float(string, "PosZ"));

				SetSpawnInfo(playerid, 1, dini_Int(file, "Skin"), dini_Int(file, "CasaX"), dini_Int(file, "CasaY"), dini_Int(file, "CasaZ"), 354.1657, 0, 0, 0, 0, 0, 0);

				format(STRX, sizeof(STRX), "{00FF00}Casa ID: {FF0000}%d\n{00FF00}Dono: {FF0000}%s\n{00FF00}Morador: {FF0000}%s\n{00FF00}Valor: {FF0000}$%d\n\n{00FF00}Último uso: {FF0000}%s", moradia[playerid], dini_Get(string, "Dono"), GetPlayerNameEx(playerid), dini_Int(string, "Preco"), dini_Get(string, "DataSet"));
				UpdateDynamic3DTextLabelText(ctextoid[moradia[playerid]], -1, STRX);

				SendClientMessage(playerid, Amarelo, "Agora você está morando em uma casa!");
				SendClientMessage(convitede[playerid], Amarelo, "O(A) jogador(a) aceitou!");
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Você não foi convidado!");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/recusarmoradia", true) == 0)
	{
		if(morar[playerid] == 1)
		{
			morar[playerid] = 0;
			moradia[playerid] = 0;
			convitede[playerid] = INVALID_PLAYER_ID;

			SendClientMessage(playerid, Amarelo, "Você recusou.");
			SendClientMessage(convitede[playerid], Amarelo, "O(A) jogador(a) recusou!");
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não foi convidado!");
		}
		return 1;
	}

	if(strcmp(cmd, "/entrarcasa", true) == 0)
	{
		new Float:X, Float:Y, Float:Z;

		for(new c = 0; c < MAX_CASAS; c++)
		{
			format(string, sizeof(string), PASTA_CASAS, c);
			if(dini_Exists(string) && IsPlayerInRangeOfPoint(playerid, 2.0, dini_Float(string, "PosX"), dini_Float(string, "PosY"), dini_Float(string, "PosZ")))
			{
				if(dini_Int(string, "Trancada") == 1)
				{
					if(strcmp(dini_Get(string, "Dono"), GetPlayerNameEx(playerid), false) == 0 || PlayerInfo[playerid][SCON] == true)
					{
						format(file, sizeof(file), PASTA_Int, dini_Int(string, "Int"));
						if(dini_Exists(file))
						{
							GetPlayerPos(playerid, X, Y, Z);
							emcasa[playerid] = 1;

							CasaX[playerid] = X;
							CasaY[playerid] = Y;
							CasaZ[playerid] = Z;

							SetPlayerVirtualWorld(playerid, c);
							SetPlayerInterior(playerid, dini_Int(file, "Int"));

							SetPlayerPos(playerid, dini_Float(file, "EX"), dini_Float(file, "EY"), dini_Float(file, "EZ"));
							SendClientMessage(playerid, Vermelho, "Você entrou na casa, para sair aperte ENTER.");
							return 1;
						}
					}
				}
				else
				{
					format(file, sizeof(file), PASTA_Int, dini_Int(string, "Int"));
					if(dini_Exists(file))
					{
						GetPlayerPos(playerid, X, Y, Z);
						emcasa[playerid] = 1;

						CasaX[playerid] = X;
						CasaY[playerid] = Y;
						CasaZ[playerid] = Z;

						SetPlayerVirtualWorld(playerid, c);
						SetPlayerInterior(playerid, dini_Int(file, "Int"));

						SetPlayerPos(playerid, dini_Float(file, "EX"), dini_Float(file, "EY"), dini_Float(file, "EZ"));
						SendClientMessage(playerid, Vermelho, "Você entrou na casa, para sair aperte ENTER.");
						return 1;
					}
				}
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/verinterior", true) == 0)
	{
		new intid, Float:X, Float:Y, Float:Z;

		if(sscanf(cmdtext, "s[13]d", cmd, intid))
		{
			SendClientMessage(playerid, Vermelho, "/verinterior [interior]");
			return 1;
		}
		if(pAdmin[playerid] == 5)
		{
			format(string, sizeof(string), PASTA_Int, intid);
			if(dini_Exists(string))
			{
				if(GetPlayerInterior(playerid) == 0)
				{
					GetPlayerPos(playerid, X, Y, Z);
					emcasa[playerid] = 1;

					CasaX[playerid] = X;
					CasaY[playerid] = Y;
					CasaZ[playerid] = Z;
				}
				SetPlayerInterior(playerid, dini_Int(string, "Int"));
				SetPlayerPos(playerid, dini_Float(string, "EX"), dini_Float(string, "EY"), dini_Float(string, "EZ"));
				SendClientMessage(playerid, Amarelo, "Para voltar aperte ENTER.");
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
			}
		}
		return 1;
	}

	// Casamento
	if(strcmp(cmd, "/pedircasamento", true) == 0)
	{
		new pid, Float:x, Float:y, Float:z;

		if(sscanf(cmdtext, "s[16]u", cmd, pid))
		{
			SendClientMessage(playerid, Vermelho, "/pedircasamento [id]");
			return 1;
		}
		if(pid == playerid)
		{
			SendClientMessage(playerid, Vermelho, "Você não pode casar com si mesmo.");
			return 1 ;
		}
		if(IsPlayerConnected(pid))
		{
			format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
			if(dini_Int(file, "Casado") == 0)
			{
				GetPlayerPos(pid, x, y, z);
				if(IsPlayerInRangeOfPoint(playerid, 10.0, x, y, z))
				{
					casar[playerid] = 1;
					pedidode[pid] = playerid;

					format(string, sizeof(string), "{FF00EE}%s {FFFFFF}está te pedindo em casamento.\n{FFFF00}Escolha abaixo a resposta para o pedido:", GetPlayerNameEx(playerid));
					ShowPlayerDialog(pid, pedidocasamento, DIALOG_STYLE_MSGBOX, "Pedido de Casamento", string, "Aceitar", "Recusar");
				}
				else
				{
					SendClientMessage(playerid, Vermelho, "Chegue mais perto para pedir.");
				}
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Você já está casado(a), divorcie antes.");
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente!");
		}
		return 1;
	}

	if(strcmp(cmd, "/divorcio", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Casado") == 1)
		{
			format(string, sizeof(string), "{FF00EE}Você está casado(a) com: %s.\n{FFFF00}Quer mesmo prosseguir com esta separação?", dini_Get(file, "CasouCom"));
			ShowPlayerDialog(playerid, divorcio, DIALOG_STYLE_MSGBOX, "Divórcio", string, "Sim", "Não");
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não está casado(a).");
		}
		return 1;
	}

	if(strcmp(cmd, "/criarportao", true) == 0)
	{
		new modelo, Float:pX, Float:pY, Float:pZ;

		if(sscanf(cmdtext, "s[13]d", cmd, modelo))
		{
			SendClientMessage(playerid, Vermelho, "/criarportao [modelo]");
			SendClientMessage(playerid, Verde, "Modelos permitidos: 969, 971, 980");
			return 1;
		}
		if(pAdmin[playerid] > 4)
		{
			GetPlayerPos(playerid, pX, pY, pZ);
			PlayerCreatePortao(playerid, modelo, pX, pY, pZ, GetPlayerInterior(playerid));
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp(cmd, "/modeloportao", true) == 0)
	{
		new modelo, portaoid;

		if(sscanf(cmdtext, "s[14]d", cmd, modelo))
		{
			SendClientMessage(playerid, Vermelho, "/modeloportao [modelo]");
			SendClientMessage(playerid, Verde, "Modelos permitidos: 969, 971, 980");
			return 1;
		}
		if(!(modelo == 969 || modelo == 971 || modelo == 980))
		{
			SendClientMessage(playerid, Vermelho, "Use um modelo válido! | Modelos: 969, 971, 980");
			return 1;
		}
		if(pAdmin[playerid] > 4)
		{
			for(new portao = 0; portao < MAX_PORTOES; portao++)
			{
				format(string, sizeof(string), PASTA_PORTOES, portao);
				if(dini_Exists(string))
				{
					if(IsPlayerInRangeOfPoint(playerid, 20.0, dini_Float(string, "fCordX"), dini_Float(string, "fCordY"), dini_Float(string, "fCordZ")))
					{
						if(dini_Int(string, "TDono") == 3)
						{
							SendClientMessage(playerid, Vermelho, "Este portão foi deletado!");
						}
						else
						{
							DestroyDynamicObject(dini_Int(string, "Id"));
							dini_IntSet(string, "Modelo", modelo);

							portaoid = CreateDynamicObject(modelo, dini_Float(string, "fCordX"), dini_Float(string, "fCordY"), dini_Float(string, "fCordZ"), dini_Float(string, "fCordRX"), dini_Float(string, "fCordRY"), dini_Float(string, "fCordRZ"), -1, -1, -1, 200.0);
							dini_IntSet(string, "Id", portaoid);
							return 1;
						}
					}
				}
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
			return 1;
		}
		return 1;
	}

	if(strcmp(cmd, "/mudarportao", true) == 0)
	{
		new id;

		if(sscanf(cmdtext, "s[13]d", cmd, id))
		{
			SendClientMessage(playerid, Vermelho, "/mudarportao [id]");
			return 1;
		}
		if(pAdmin[playerid] > 4)
		{
			format(string, sizeof(string), PASTA_PORTOES, id);
			if(dini_Exists(string))
			{
				if(IsPlayerInRangeOfPoint(playerid, 20.0, dini_Float(string, "fCordX"), dini_Float(string, "fCordY"), dini_Float(string, "fCordZ")))
				{
					SetPVarInt(playerid, "pidToEdit", id);
					SetPVarInt(playerid, "objToEdit", dini_Int(string, "Id"));
					SetPVarInt(playerid, "pidSalvo", 1);

					ShowPlayerDialog(playerid, portaoeditor, DIALOG_STYLE_LIST, "Editando Portão", "Editar X Posição\nEditar Y Posição\nEditar Z Posição\nEditar RX Posição\nEditar RY Posição\nEditar RZ Posição\nDeletar Edição\nSalvar Edição", "OK", "Voltar");
				}
				else
				{
					SendClientMessage(playerid, Vermelho, "Você está muito longe do portão!");
				}
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente!");
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp(cmd, "/deletarportao", true) == 0)
	{
		new portaoid;

		if(sscanf(cmdtext, "s[15]d", cmd, portaoid))
		{
			SendClientMessage(playerid, Vermelho, "/deletarportao [portao]");
			return 1;
		}
		if(pAdmin[playerid] > 4)
		{
			format(string, sizeof(string), PASTA_PORTOES, portaoid);
			if(dini_Exists(string))
			{
				if(dini_Int(string, "TDono") == 3)
				{
					SendClientMessage(playerid, Vermelho, "Este portão já foi deletado!");
				}
				else
				{
					DestroyDynamic3DTextLabel(potextoid[portaoid]);
					potextoid[portaoid] = Text3D:INVALID_3DTEXT_ID;

					DestroyDynamicObject(dini_Int(string, "Id"));

					dini_IntSet(string, "TDono", 3);
					dini_Set(string, "Dono", "Ninguem");

					dini_Set(string, "Convidado1", "Ninguem");
					dini_Set(string, "Convidado2", "Ninguem");
					dini_Set(string, "Convidado3", "Ninguem");

					dini_IntSet(string, "Id", INVALID_OBJECT_ID);
					SendClientMessage(playerid, roxo, "Portão deletado com sucesso!");
				}
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp(cmd, "/portoesdeletados", true) == 0)
	{
		new count = 0;

		SendClientMessage(playerid, 0x008000AA, "=== Portões Deletados ===");
		for(new portao = 0; portao < MAX_PORTOES; portao++)
		{
			format(file, sizeof(file), PASTA_PORTOES, portao);
			if(dini_Int(file, "TDono") == 3)
			{
				format(string, sizeof(string), "» Portão ID: %d - Deletado", portao);
				SendClientMessage(playerid, 0x0088CAAA, string);
				count++;
			}
		}
		if(count == 0)
		{
			SendClientMessage(playerid, 0xFF0000AA, "Nenhum portão foi deletado!");
		}
		return 1;
	}

	if(strcmp(cmd, "/infocarro", true) == 0)
	{
		for(new carro = 0; carro < MAX_CONCES; carro++)
		{
			format(file, sizeof(file), PASTA_CONCE, carro);
			if(dini_Exists(file))
			{
				if(IsPlayerInRangeOfPoint(playerid, 3.0, dini_Float(file, "CordX"), dini_Float(file, "CordY"), dini_Float(file, "CordZ")))
				{
					format(string, sizeof(string), "Veículo: %s | Conce ID: %d", GetVehicleName(dini_Int(file, "Id")), carro);
					SendClientMessage(playerid, Blue, string);

					format(string, sizeof(string), "Último uso: %s | Dono: %s | Valor: %d", dini_Get(file, "DataSet"), dini_Get(file, "Dono"), dini_Int(file, "Preco"));
					SendClientMessage(playerid, Blue, string);

					format(string, sizeof(string), "Cópias da chave com: %s, %s, %s", dini_Get(file, "Convidado1"), dini_Get(file, "Convidado2"), dini_Get(file, "Convidado3"));
					SendClientMessage(playerid, Blue, string);
					return 1;
				}
				else if(IsPlayerInAnyVehicle(playerid) && GetPlayerVehicleID(playerid) == dini_Int(file, "Id"))
				{
					format(string, sizeof(string), "Veículo: %s | Conce ID: %d", GetVehicleName(dini_Int(file, "Id")), carro);
					SendClientMessage(playerid, Blue, string);

					format(string, sizeof(string), "Último uso: %s | Dono: %s | Valor: %d", dini_Get(file, "DataSet"), dini_Get(file, "Dono"), dini_Int(file, "Preco"));
					SendClientMessage(playerid, Blue, string);

					format(string, sizeof(string), "Cópias da chave com: %s, %s, %s", dini_Get(file, "Convidado1"), dini_Get(file, "Convidado2"), dini_Get(file, "Convidado3"));
					SendClientMessage(playerid, Blue, string);
					return 1;
				}
			}
		}
		if(IsPlayerInAnyVehicle(playerid))
		{
			SendClientMessage(playerid, Vermelho, "Este veículo parece não ter sido fornecido pela concessionária.");
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Não consegui mostrar as informações, entre no veículo para ver e tente novamente.");
		}
		return 1;
	}

	if(strcmp(cmd, "/infoportao", true) == 0)
	{
		for(new portao = 0; portao < MAX_PORTOES; portao++)
		{
			format(file, sizeof(file), PASTA_PORTOES, portao);
			if(dini_Exists(file))
			{
				if(IsPlayerInRangeOfPoint(playerid, 20.0, dini_Float(file, "fCordX"), dini_Float(file, "fCordY"), dini_Float(file, "fCordZ")))
				{
					format(string, sizeof(string), "Portão ID: %d | Dono: %s", portao, dini_Get(file, "Dono"));
					SendClientMessage(playerid, Blue, string);

					format(string, sizeof(string), "Cópias da chave com: %s, %s, %s", dini_Get(file, "Convidado1"), dini_Get(file, "Convidado2"), dini_Get(file, "Convidado3"));
					SendClientMessage(playerid, Blue, string);
					return 1;
				}
			}
		}
		SendClientMessage(playerid, Vermelho, "Não tem nenhum portão válido por perto!");
		return 1;
	}

	if(strcmp(cmd, "/irportao", true) == 0)
	{
		new id;

		if(sscanf(cmdtext, "s[10]d", cmd, id))
		{
			SendClientMessage(playerid, Vermelho, "/irportao [id]");
			return 1;
		}
		if(pAdmin[playerid] > 3)
		{
			format(string, sizeof(string), PASTA_PORTOES, id);
			if(dini_Exists(string))
			{
				SetPlayerInterior(playerid, dini_Int(string, "IntID"));
				SetPlayerPos(playerid, dini_Float(string, "fCordX")+2, dini_Float(string, "fCordY")+2, dini_Float(string, "fCordZ"));

				TogglePlayerControllable(playerid, false);
				SetTimerEx("DestogglePlayerControllable", 1000, 0, "d", playerid);
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp(cmd, "/darchave", true) == 0)
	{
		new plid, id;

		if(sscanf(cmdtext, "s[10]ud", cmd, plid, id))
		{
			SendClientMessage(playerid, Vermelho, "/darchave [id] [portão-id]");
			return 1;
		}
		if(pAdmin[playerid] > 1)
		{
			format(file, sizeof(file), PASTA_PORTOES, id);
			if(dini_Exists(file))
			{
				if(dini_Int(file, "TDono") == 0)
				{
					dini_Set(file, "Dono", GetPlayerNameEx(plid));
					dini_IntSet(file, "TDono", 1);

					format(string, sizeof(string), "O(A) ADM %s te deu a chave do portão: %d", GetPlayerNameEx(playerid), id);
					SendClientMessage(plid, Blue, string);

					SendClientMessage(playerid, Verde, "Comando efetuado com sucesso!");
				}
				else
				{
					SendClientMessage(playerid, Vermelho, "Este portão já tem dono!");
				}
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Portão inválido, tente novamente!");
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp(cmd, "/copiachave", true) == 0)
	{
		new plid, id;

		if(sscanf(cmdtext, "s[12]ud", cmd, plid, id))
		{
			SendClientMessage(playerid, Vermelho, "/copiachave [id] [portão-id]");
			return 1;
		}
		format(file, sizeof(file), PASTA_PORTOES, id);
		if(dini_Exists(file))
		{
			if(strcmp(dini_Get(file, "Dono"), GetPlayerNameEx(playerid), false) == 0 || pAdmin[playerid] >= 2)
			{
				if(strcmp(dini_Get(file, "Convidado1"), "Ninguem", false) == 0)
				{
					dini_Set(file, "Convidado1", GetPlayerNameEx(plid));

					format(string, sizeof(string), "%s te deu uma cópia da chave do portão: %d", GetPlayerNameEx(playerid), id);
					SendClientMessage(plid, Blue, string);

					format(string, sizeof(string), "Cópia gerada com sucesso, agora %s pode abrir seu portão!", GetPlayerNameEx(plid));
					SendClientMessage(playerid, Verde, string);
					return 1;
				}
				if(strcmp(dini_Get(file, "Convidado2"), "Ninguem", false) == 0)
				{
					dini_Set(file, "Convidado2", GetPlayerNameEx(plid));

					format(string, sizeof(string), "%s te deu uma cópia da chave do portão: %d", GetPlayerNameEx(playerid), id);
					SendClientMessage(plid, Blue, string);

					format(string, sizeof(string), "Cópia gerada com sucesso, agora %s pode abrir seu portão!", GetPlayerNameEx(plid));
					SendClientMessage(playerid, Verde, string);
					return 1;
				}
				if(strcmp(dini_Get(file, "Convidado3"), "Ninguem", false) == 0)
				{
					dini_Set(file, "Convidado3", GetPlayerNameEx(plid));

					format(string, sizeof(string), "%s te deu uma cópia da chave do portão: %d", GetPlayerNameEx(playerid), id);
					SendClientMessage(plid, Blue, string);

					format(string, sizeof(string), "Cópia gerada com sucesso, agora %s pode abrir seu portão!", GetPlayerNameEx(plid));
					SendClientMessage(playerid, Verde, string);
					return 1;
				}
				SendClientMessage(playerid, Vermelho, "O limite de chaves foi ecedido!");
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Você não é dono deste portão.");
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Portão inválido, tente novamente!");
		}
		return 1;
	}

	if(strcmp(cmd, "/tomarchave", true) == 0)
	{
		new nick[MAX_PLAYER_NAME], id;

		if(sscanf(cmdtext, "s[12]s[24]d", cmd, nick, id))
		{
			SendClientMessage(playerid, Vermelho, "/tomarchave [nick] [portão-id]");
			return 1;
		}
		format(file, sizeof(file), PASTA_PORTOES, id);
		if(dini_Exists(file))
		{
			if(strcmp(dini_Get(file, "Dono"), GetPlayerNameEx(playerid), false) == 0 || pAdmin[playerid] >= 2)
			{
				if(strcmp(dini_Get(file, "Convidado1"), nick, false) == 0)
				{
					dini_Set(file, "Convidado1", "Ninguem");

					format(string, sizeof(string), "Você tomou a chave de %s agora ele(a) não pode abrir seu portão!", nick);
					SendClientMessage(playerid, Verde, string);
					return 1;
				}
				if(strcmp(dini_Get(file, "Convidado2"), nick, false) == 0)
				{
					dini_Set(file, "Convidado2", "Ninguem");

					format(string, sizeof(string), "Você tomou a chave de %s agora ele(a) não pode abrir seu portão!", nick);
					SendClientMessage(playerid, Verde, string);
					return 1;
				}
				if(strcmp(dini_Get(file, "Convidado3"), nick, false) == 0)
				{
					dini_Set(file, "Convidado3", "Ninguem");

					format(string, sizeof(string), "Você tomou a chave de %s agora ele(a) não pode abrir seu portão!", nick);
					SendClientMessage(playerid, Verde, string);
					return 1;
				}
				SendClientMessage(playerid, Vermelho, "Verifique se digitou o nick corretamente!");
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Você não é dono deste portão.");
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Portão inválido, tente novamente!");
		}
		return 1;
	}

	if(strcmp(cmd, "/liberarportao", true) == 0)
	{
		new id, portaoid;

		if(sscanf(cmdtext, "s[15]d", cmd, id))
		{
			SendClientMessage(playerid, Vermelho, "/liberarportao [id]");
			return 1;
		}
		if(pAdmin[playerid] > 1)
		{
			format(file, sizeof(file), PASTA_PORTOES, id);
			if(dini_Exists(file))
			{
				if(!(dini_Int(file, "TDono") == 3))
				{
					DestroyDynamic3DTextLabel(potextoid[id]);
					potextoid[id] = Text3D:INVALID_3DTEXT_ID;

					DestroyDynamicObject(dini_Int(file, "Id"));
				}
				portaoid = CreateDynamicObject(dini_Int(file, "Modelo"), dini_Float(file, "fCordX"), dini_Float(file, "fCordY"), dini_Float(file, "fCordZ"), dini_Float(file, "fCordRX"), dini_Float(file, "fCordRY"), dini_Float(file, "fCordRZ"), -1, -1, -1, 200.0);
				dini_IntSet(file, "Id", portaoid);

				format(STRX, sizeof(STRX), "{00FF00}/ap %d {FF0000}para abrir\n{00FF00}/fp %d {FF0000}para fechar", id, id);
				potextoid[id] = CreateDynamic3DTextLabel(STRX, -1, dini_Float(file, "fCordX"), dini_Float(file, "fCordY"), dini_Float(file, "fCordZ"), 30.0, INVALID_PLAYER_ID, INVALID_VEHICLE_ID, 1, -1, -1, -1, 200.0);

				dini_Set(file, "Dono", "Ninguem");
				dini_Set(file, "Convidado1", "Ninguem");
				dini_Set(file, "Convidado2", "Ninguem");
				dini_Set(file, "Convidado3", "Ninguem");
				dini_IntSet(file, "TDono", 0);

				format(string, sizeof(string), "O(A) ADM %s liberou o portão: %d", GetPlayerNameEx(playerid), id);
				SendClientMessageToAll(Blue, string);
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Portão inválido, tente novamente!");
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp(cmd, "/ap", true) == 0)
	{
		new id;

		if(sscanf(cmdtext, "s[4]d", cmd, id))
		{
			SendClientMessage(playerid, Vermelho, "/ap [id]");
			return 1;
		}
		format(file, sizeof(file), PASTA_PORTOES, id);
		if(dini_Exists(file))
		{
			if(!IsPlayerInRangeOfPoint(playerid, 50.0, dini_Float(file, "fCordX"), dini_Float(file, "fCordY"), dini_Float(file, "fCordZ")))
			{
				SendClientMessage(playerid, Vermelho, "Você está muito longe do portão!");
				return 1;
			}
			if(dini_Int(file, "TDono") == 3)
			{
				SendClientMessage(playerid, Vermelho, "Este portão foi deletado!");
			}
			else
			{
				if(strcmp(dini_Get(file, "Dono"), GetPlayerNameEx(playerid), false) == 0 || strcmp(dini_Get(file, "Convidado1"), GetPlayerNameEx(playerid), false) == 0 || strcmp(dini_Get(file, "Convidado2"), GetPlayerNameEx(playerid), false) == 0 || strcmp(dini_Get(file, "Convidado3"), GetPlayerNameEx(playerid), false) == 0 || pAdmin[playerid] > 2)
				{
					MoveDynamicObject(dini_Int(file, "Id"), dini_Float(file, "aCordX"), dini_Float(file, "aCordY"), dini_Float(file, "aCordZ"), 4.0);
				}
				else
				{
					SendClientMessage(playerid, Vermelho, "Você não tem a chave!");
				}
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Portão inválido, tente novamente!");
		}
		return 1;
	}

	if(strcmp(cmd, "/fp", true) == 0)
	{
		new id;

		if(sscanf(cmdtext, "s[4]d", cmd, id))
		{
			SendClientMessage(playerid, Vermelho, "/fp [id]");
			return 1;
		}
		format(file, sizeof(file), PASTA_PORTOES, id);
		if(dini_Exists(file))
		{
			if(!IsPlayerInRangeOfPoint(playerid, 50.0, dini_Float(file, "aCordX"), dini_Float(file, "aCordY"), dini_Float(file, "aCordZ")))
			{
				SendClientMessage(playerid, Vermelho, "Você está muito longe do portão!");
				return 1;
			}
			if(dini_Int(file, "TDono") == 3)
			{
				SendClientMessage(playerid, Vermelho, "Este portão foi deletado!");
			}
			else
			{
				if(strcmp(dini_Get(file, "Dono"), GetPlayerNameEx(playerid), false) == 0 || strcmp(dini_Get(file, "Convidado1"), GetPlayerNameEx(playerid), false) == 0 || strcmp(dini_Get(file, "Convidado2"), GetPlayerNameEx(playerid), false) == 0 || strcmp(dini_Get(file, "Convidado3"), GetPlayerNameEx(playerid), false) == 0 || pAdmin[playerid] > 2)
				{
					MoveDynamicObject(dini_Int(file, "Id"), dini_Float(file, "fCordX"), dini_Float(file, "fCordY"), dini_Float(file, "fCordZ"), 4.0);
				}
				else
				{
					SendClientMessage(playerid, Vermelho, "Você não tem a chave!");
				}
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Portão inválido, tente novamente!");
		}
		return 1;
	}

	if(strcmp("/pm", cmd, true) == 0)
	{
		new id, Message[128], gMessage[128], pMessage[128];

		if(sscanf(cmdtext, "s[4]us[128]", cmd, id, pMessage))
		{
			SendClientMessage(playerid, tcadm, "Use: /pm [id] [mensagem]");
			return 1;
		}
		if(!IsPlayerConnected(id))
		{
			SendClientMessage(playerid, tcadm, "Valor inválido, tente novamente!");
			return 1;
		}
		for(new i = 0; i < strlen(pMessage); i++)
		{
			gMessage[i] = pMessage[i];
		}
		gMessage[strlen(pMessage)] = EOS;

		if(!(pAdmin[playerid] > 0))
		{
			for(new p = 0; p < sizeof Palavroes; p++)
			{
				new fp = strfind(gMessage, Palavroes[p], true);
				while(fp != -1)
				{
					for(new i = 0; i < strlen(Palavroes[p]); i++)
					{
						gMessage[fp + i] = '_';
					}
					fp = strfind(gMessage, Palavroes[p], true);
				}
			}
			if(VBIsIP(gMessage))
			{
				SendClientMessage(playerid, Amarelo, "|_ ANTI-SPAM _| Você não pode fazer spam no servidor.");
				return 1;
			}
		}
		if(playerid != id)
		{
			if(blockpm[id] == 1)
			{
				SendClientMessage(playerid, tcadm, "Este ADM não está recebendo PM's.");
				return 1;
			}
			format(Message, sizeof(Message), "Mensagem enviada para %s (ID: %d): %s", GetPlayerNameEx(id), id, gMessage);
			SendClientMessage(playerid, 0xFFD700AA, Message);

			format(Message, sizeof(Message), "Mensagem recebida de %s (ID: %d): %s", GetPlayerNameEx(playerid), playerid, gMessage);
			SendClientMessage(id, 0xDAA520AA, Message);

			format(Message, sizeof(Message), "PM: %s (%d) > %s (%d): %s", GetPlayerNameEx(playerid), playerid, GetPlayerNameEx(id), id, gMessage);
			ABroadCastPM(Amarelo, Message);

			PlayerPlaySound(id, 1085, 0.0, 0.0, 0.0);
			printf("PM de %s para %s: %s", GetPlayerNameEx(playerid), GetPlayerNameEx(id), gMessage);
		}
		else
		{
			SendClientMessage(playerid, tcadm, "Você não pode enviar PM para você mesmo!");
		}
		return 1;
	}

	if(strcmp(cmd, "/fianca", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Preso") == 1)
		{
			if(dini_Int(file, "SaldoBancario") > 10000)
			{
				if(Preso[playerid] >= 2)
				{
					SoltarPlayer(playerid);
					dini_IntSet(file, "SaldoBancario", dini_Int(file, "SaldoBancario")-Fianca);

					SendClientMessage(playerid, Verde, "Você pagou a fiança e foi solto. O dinheiro foi retirado do banco.");
				}
				else
				{
					SendClientMessage(playerid, Vermelho, "Você tem que esperar 2 minutos para poder pagar a fiança!");
				}
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Você não possui dinheiro suficiente! ($10000)");
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não está preso!");
		}
		return 1;
	}

	if(strcmp("/ccar", cmd, true) == 0)
	{
		if(pAdmin[playerid] == 5)
		{
			new carid, preco, cor1, cor2,
				Float:X, Float:Y, Float:Z, Float:ang;

			if(sscanf(cmdtext, "s[6]ddD(-1)D(-1)", cmd, carid, preco, cor1, cor2))
			{
				SendClientMessage(playerid, Cinza, "Use: /ccar [id] [preço] [cor1] [cor2]");
				return 1;
			}
			if(carid >= 400 && carid <= 611)
			{
				if(carid == 425 || carid == 469 || carid == 538 || carid == 537 || carid == 520 || carid == 449 || carid == 447 || carid == 569 || carid == 570 || carid == 432)
				{
					SendClientMessage(playerid, tcadm, "Veículo proibido, tente outro! | ID's = 400-611");
					return 1;
				}
				if(IsPlayerInAnyVehicle(playerid))
				{
					GetPlayerPos(playerid, X, Y, Z);
					GetVehicleZAngle(GetPlayerVehicleID(playerid), ang);
					PlayerAddConceVehicle(playerid, carid, preco, X, Y, Z, ang, cor1, cor2);
				}
				else
				{
					GetPlayerPos(playerid, X, Y, Z);
					GetPlayerFacingAngle(playerid, ang);
					PlayerAddConceVehicle(playerid, carid, preco, X, Y, Z, ang, cor1, cor2);
				}
			}
			else
			{
				SendClientMessage(playerid, Cinza, "Valor inválido, tente novamente! | ID's = 400-611");
			}
		}
		else
		{
			SendClientMessage(playerid, Cinza, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp("/comprarcarro", cmd, true) == 0)
	{
		new conceid;

		if(sscanf(cmdtext, "s[14]d", cmd, conceid))
		{
			if(IsPlayerInAnyVehicle(playerid))
			{
				for(new carro = 0; carro < MAX_CONCES; carro++)
				{
					format(file, sizeof(file), PASTA_CONCE, carro);
					if(dini_Exists(file))
					{
						if(GetPlayerVehicleSeat(playerid) == 0 && GetPlayerVehicleID(playerid) == dini_Int(file, "Id"))
						{
							if(dini_Int(file, "TDono") == 0)
							{
								if(GetPlayerGrana(playerid) >= dini_Int(file, "Preco"))
								{
									if(GetCarros(playerid) < MAX_PLAYER_CONCE)
									{
										dini_IntSet(file, "TDono", 1);
										dini_Set(file, "Dono", GetPlayerNameEx(playerid));
										if(!IsPlayerVIP(playerid))
										{
											GivePlayerGrana(playerid, -dini_Int(file, "Preco"));
											TogglePlayerControllable(playerid, 1);
											dini_IntSet(file, "CarVIP", 0);
											intest[playerid] = 0;

											format(string, sizeof(string), "%s comprou um carro, deve estar feliz!", GetPlayerNameEx(playerid));
											SendClientMessageToAll(roxo, string);
											ClearChatbox(playerid, 1);

											SendClientMessage(playerid, Azul, "  Veículo comprado com sucesso!");
											SendClientMessage(playerid, Azul, "  Para ver os comandos do veículo, use: /meucarro");
											SendClientMessage(playerid, LARANJA, "|___________________________________________________________|");
										}
										else
										{
											GivePlayerGrana(playerid, -dini_Int(file, "Preco"));
											TogglePlayerControllable(playerid, 1);
											dini_IntSet(file, "CarVIP", 1);
											intest[playerid] = 0;

											format(string, sizeof(string), "%s comprou um carro equipado com alarme explosivo.", GetPlayerNameEx(playerid));
											SendClientMessageToAll(roxo, string);
											ClearChatbox(playerid, 1);

											SendClientMessage(playerid, Azul, "  Veículo comprado com sucesso!");
											SendClientMessage(playerid, Amarelo, "  (VIP) Seu carro foi equipado com alarme explosivo.");
											SendClientMessage(playerid, Azul, "  Para ver os comandos do veículo, use: /meucarro");
											SendClientMessage(playerid, LARANJA, "|___________________________________________________________|");
										}
										return 1;
									}
									else
									{
										SendClientMessage(playerid, Amarelo, "Você só pode ter "#MAX_PLAYER_CONCE" carros!");
										SendClientMessage(playerid, Vermelho, "Para comprar outro venda um de seus!");
										return 1;
									}
								}
								else
								{
									SendClientMessage(playerid, Vermelho, "Você não tem dinheiro suficiente!");
									return 1;
								}
							}
							else
							{
								SendClientMessage(playerid, Vermelho, "Este veículo não está a venda!");
								return 1;
							}
						}
					}
				}
			}
			SendClientMessage(playerid, Vermelho, "Use /comprarcarro [conceid]");
			return 1;
		}
		format(file, sizeof(file), PASTA_CONCE, conceid);
		if(dini_Exists(file))
		{
			if(dini_Int(file, "TDono") == 0)
			{
				if(GetPlayerGrana(playerid) >= dini_Int(file, "Preco"))
				{
					if(GetCarros(playerid) < MAX_PLAYER_CONCE)
					{
						dini_IntSet(file, "TDono", 1);
						dini_Set(file, "Dono", GetPlayerNameEx(playerid));
						if(!IsPlayerVIP(playerid))
						{
							GivePlayerGrana(playerid, -dini_Int(file, "Preco"));
							TogglePlayerControllable(playerid, 1);
							dini_IntSet(file, "CarVIP", 0);
							intest[playerid] = 0;

							format(string, sizeof(string), "%s comprou um carro, deve estar feliz!", GetPlayerNameEx(playerid));
							SendClientMessageToAll(roxo, string);
							ClearChatbox(playerid, 1);

							SendClientMessage(playerid, Azul, "  Veículo comprado com sucesso!");
							SendClientMessage(playerid, Azul, "  Para ver os comandos do veículo, use: /meucarro");
							SendClientMessage(playerid, LARANJA, "|___________________________________________________________|");
						}
						else
						{
							GivePlayerGrana(playerid, -dini_Int(file, "Preco"));
							TogglePlayerControllable(playerid, 1);
							dini_IntSet(file, "CarVIP", 1);
							intest[playerid] = 0;

							format(string, sizeof(string), "%s comprou um carro equipado com alarme explosivo.", GetPlayerNameEx(playerid));
							SendClientMessageToAll(roxo, string);
							ClearChatbox(playerid, 1);

							SendClientMessage(playerid, Azul, "  Veículo comprado com sucesso!");
							SendClientMessage(playerid, Amarelo, "  (VIP) Seu carro foi equipado com alarme explosivo.");
							SendClientMessage(playerid, Azul, "  Para ver os comandos do veículo, use: /meucarro");
							SendClientMessage(playerid, LARANJA, "|___________________________________________________________|");
						}
					}
					else
					{
						SendClientMessage(playerid, Amarelo, "Você só pode ter "#MAX_PLAYER_CONCE" carros!");
						SendClientMessage(playerid, Vermelho, "Para comprar outro venda um de seus!");
					}
				}
				else
				{
					SendClientMessage(playerid, Vermelho, "Você não tem dinheiro suficiente!");
				}
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Este veículo não está a venda!");
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Conce ID inválido, este veículo não existe ainda!");
		}
		return 1;
	}

	if(strcmp(cmd, "/testdrive", true) == 0)
	{
		if(incar[playerid] == 1)
		{
			incar[playerid] = 0;
			intest[playerid] = 1;

			TogglePlayerControllable(playerid, 1);
			testtime = SetTimerEx("TestDrive", 0, false, "e", playerid);

			SendClientMessage(playerid, Azul, "O Test-Drive não pôde ser iniciado!");
			SendClientMessage(playerid, Blue, "Test-Drive desabilitado pelo administrador!");
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Voçê não está em um carro à venda!");
		}
		return 1;
	}

	if(strcmp(cmd, "/meucarro", true) == 0)
	{
		for(new carro = 0; carro < MAX_CONCES; carro++)
		{
			format(string, sizeof(string), PASTA_CONCE, carro);
			if(dini_Exists(string))
			{
				if(strcmp(dini_Get(string, "Dono"), GetPlayerNameEx(playerid), false) == 0)
				{
					ShowPlayerDialog(playerid, 4501, DIALOG_STYLE_LIST, "Meu Carro", "{FF00FF}Entrar\n{FF0000}Salvar\n{FFDD00}Chaves\n{AFAFAF}Cor 1\n{33AA33}Cor 2\n{FFFFFF}Respawnar\n\n{2C0BB3}Ferramentas\n{00FFFF}Vender\n{00FF00}Tunar\n{FF33FF}Destunar", "OK", "Cancelar");
					SendClientMessage(playerid, Verde, "Você está no menu de seu(s) carro(s).");
					return 1;
				}
			}
		}
		SendClientMessage(playerid, Verde, "Você não tem nenhum carro proprio.");
		return 1;
	}

	if(strcmp(cmd, "/modelocarro", true) == 0)
	{
		if(pAdmin[playerid] > 4)
		{
			new modelo, Float:carroX, Float:carroY, Float:carroZ, Float:carroA;

			if(sscanf(cmdtext, "s[13]d", cmd, modelo))
			{
				SendClientMessage(playerid, Vermelho, "/modelocarro [modelo]");
				return 1;
			}
			if(!IsPlayerInAnyVehicle(playerid))
			{
				SendClientMessage(playerid, Vermelho, "Você não está em um veículo!");
				return 1;
			}
			if(!(modelo >= 400 && modelo <= 611))
			{
				SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente! | ID's = 400-611");
				return 1;
			}
			for(new c = 0; c < MAX_CONCES; c++)
			{
				format(string, sizeof(string), PASTA_CONCE, c);
				if(dini_Exists(string))
				{
					if(GetPlayerVehicleID(playerid) == dini_Int(string, "Id"))
					{
						GetVehiclePos(GetPlayerVehicleID(playerid), carroX, carroY, carroZ);
						GetVehicleZAngle(GetPlayerVehicleID(playerid), carroA);

						dini_IntSet(string, "Modelo", modelo);
						DestroyVehicle(dini_Int(string, "Id"));

						CriarVeiculo3(c, modelo, carroX, carroY, carroZ, carroA, dini_Int(string, "Cor1"), dini_Int(string, "Cor2"));
						SendClientMessage(playerid, Verde, "Modelo alterado com sucesso!");
						return 1;
					}
				}
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp(cmd, "/veiculosdeletados", true) == 0)
	{
		new count = 0, msg[1000];

		SendClientMessage(playerid, 0x008000AA, "=== Veículos Deletados ===");
		for(new carro = 0; carro < MAX_CONCES; carro++)
		{
			format(file, sizeof(file), PASTA_CONCE, carro);
			if(dini_Int(file, "TDono") == 3)
			{
				format(msg, sizeof(msg), "» Veículo ID: %d - Deletado", carro);
				SendClientMessage(playerid, 0x0088CAAA, msg);
				count++;
			}
		}
		if(count == 0)
		{
			SendClientMessage(playerid, 0xFF0000AA, "Nenhum veículo foi deletado!");
		}
		return 1;
	}

	if(strcmp(cmd, "/respawnccar", true) == 0)
	{
		if(pAdmin[playerid] > 4)
		{
			new carroid;

			for(new c = 0; c < MAX_CONCES; c++)
			{
				format(string, sizeof(string), PASTA_CONCE, c);
				if(dini_Exists(string))
				{
					if(GetPlayerVehicleID(playerid) == dini_Int(string, "Id"))
					{
						DestroyVehicle(dini_Int(string, "Id"));

						carroid = AddStaticVehicle(dini_Int(string, "Modelo"), dini_Float(string, "CordX"), dini_Float(string, "CordY"), dini_Float(string, "CordZ"), dini_Float(string, "Angulo"), dini_Int(string, "Cor1"), dini_Int(string, "Cor2"));
						dini_IntSet(string, "Id", carroid);
						return 1;
					}
				}
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp(cmd, "/salvarcar", true) == 0)
	{
		if(pAdmin[playerid] > 4)
		{
			new Float:carroX, Float:carroY, Float:carroZ, Float:carroA;

			if(IsPlayerInAnyVehicle(playerid))
			{
				for(new carro = 0; carro < MAX_CONCES; carro++)
				{
					format(string, sizeof(string), PASTA_CONCE, carro);
					if(dini_Exists(string))
					{
						GetVehiclePos(GetPlayerVehicleID(playerid), carroX, carroY, carroZ);
						GetVehicleZAngle(GetPlayerVehicleID(playerid), carroA);
						if(dini_Int(string, "Id") == GetPlayerVehicleID(playerid))
						{
							dini_FloatSet(string, "CordX", Float:carroX);
							dini_FloatSet(string, "CordY", Float:carroY);
							dini_FloatSet(string, "CordZ", Float:carroZ);
							dini_FloatSet(string, "Angulo", Float:carroA);

							DestroyVehicle(dini_Int(string, "Id"));

							CriarVeiculo3(carro, dini_Int(string, "Modelo"), dini_Float(string, "CordX"), dini_Float(string, "CordY"), dini_Float(string, "CordZ"), dini_Float(string, "Angulo"), dini_Int(string, "Cor1"), dini_Int(string, "Cor2"));
							SendClientMessage(playerid, Verde, "Veículo salvo na sua posição.");
							return 1;
						}
					}
				}
				SendClientMessage(playerid, Amarelo, "Este veículo não foi feito na Conce.");
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Você precisa estar no veículo.");
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp(cmd, "/on", true) == 0)
	{
		if(AFK[playerid] == 1)
		{
			AFK[playerid] = 0;
			
			TogglePlayerControllable(playerid, 1);

			TextDrawHideForPlayer(playerid, AfkText);
			TextDrawHideForPlayer(playerid, AfkBackText);

			format(string, sizeof(string), "(ANTI-AFK) %s está de volta! ( /on )", GetPlayerNameEx(playerid));
			SendClientMessageToAll(0x2BFF95AA, string);
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não está AFK!");
		}
		return 1;
	}

	if(AFK[playerid] == 1) return SendClientMessage(playerid, Vermelho, "Você está AFK e não poderá usar nenhum comando!");

	if(strcmp("/ajudavotacao", cmd, true) == 0)
	{
		if(pAdmin[playerid] > 0)
		{
			SendClientMessage(playerid, 0x008000AA, "Use: /votacao - Para iniciar uma votação.");
			SendClientMessage(playerid, 0x008000AA, "Use: /encerrar - Para encerrar uma votação.");
			return 1;
		}
	}

	if(strcmp(cmd, "/votacao", true) == 0)
	{
		new pergunta[256];

		if(sscanf(cmdtext, "s[9]s[256]", cmd, pergunta))
		{
			return SendClientMessage(playerid, 0xFFFFFFAA, "Use: /votacao [pergunta]");
		}
		if(pAdmin[playerid] > 0)
		{
			if(!votacao[iniciada])
			{
				votacao[iniciada] = true;
				votacao[sim] = 0;
				votacao[nao] = 0;

				SendClientMessageToAll(LARANJA, "|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|");
				format(string, sizeof string, "Pergunta: %s", pergunta);
				SendClientMessageToAll(VERDECLARO, string);

				SendClientMessageToAll(LARANJA, "---------------------------------------------");
				SendClientMessageToAll(VERDEMEDIO, "--> Se você acha que sim digite: /sim");
				SendClientMessageToAll(VERDEMEDIO, "--> Se você acha que não digite: /nao");
				SendClientMessageToAll(LARANJA, "|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|");

				GameTextForAll("~r~Enquete ~p~Iniciada!", 6000, 3);

				for(new i = 0; i < MAX_PLAYERS; i++)
				{
					votou[i] = false;
				}
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Já existe uma votação em andamento!");
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp(cmd, "/sim", true) == 0)
	{
		if(votou[playerid] == 1)
		{
			return SendClientMessage(playerid, Vermelho, "Você já votou.");
		}
		if(votacao[iniciada] && !votou[playerid])
		{
			votacao[sim]++;
			votacao[total]++;
			votou[playerid] = true;

			SendClientMessage(playerid, LARANJA, "Seu voto foi computado com sucesso.");
			return 1;
		}
		return 1;
	}

	if(strcmp(cmd, "/nao", true) == 0)
	{
		if(votou[playerid] == 1)
		{
			return SendClientMessage(playerid, Vermelho, "Você já votou.");
		}
		if(votacao[iniciada] && !votou[playerid])
		{
			votacao[nao]++;
			votacao[total]++;
			votou[playerid] = true;

			SendClientMessage(playerid, LARANJA, "Seu voto foi computado com sucesso.");
			return 1;
		}
		return 1;
	}

	if(strcmp(cmd, "/encerrar", true) == 0)
	{
		if(pAdmin[playerid] > 0)
		{
			if(votacao[iniciada])
			{
				SendClientMessageToAll(LARANJA, "=========== RESULTADOS ============");
				SendClientMessageToAll(LARANJA, "» RESULTADO DA VOTAÇÃO «");

				if(votacao[sim] == votacao[nao])
				{
					SendClientMessageToAll(VERMELHO, "==> EMPATE!");
				}
				else if(votacao[sim] > votacao[nao])
				{
					SendClientMessageToAll(VERMELHO, "==> A maioria votou em sim.");
				}
				else if(votacao[sim] < votacao[nao])
				{
					SendClientMessageToAll(VERMELHO, "==> A maioria votou em não.");
				}

				format(string, sizeof string, "» %d jogador(es) votaram em sim.", votacao[sim]);
				SendClientMessageToAll(VERDEMEDIO, string);

				format(string, sizeof string, "» %d jogador(es) votaram em não.", votacao[nao]);
				SendClientMessageToAll(VERDEMEDIO, string);

				format(string, sizeof string, "» Esta enquete teve %d votos!",votacao[total]);
				SendClientMessageToAll(BRANCO, string);

				votacao[iniciada] = false;
				votacao[sim] = 0;
				votacao[nao] = 0;
				votacao[total] = 0;

				for(new i = 0; i < MAX_PLAYERS; i++)
				{
					votou[i] = false;
				}

				SendClientMessageToAll(LARANJA, "===================================");
				GameTextForAll("~r~Votacao ~r~encerrada!", 6000, 3);
			}
			else
			{
				SendClientMessage(playerid, BRANCO, "Não existe nenhuma votação em andamento...");
			}
		}
		else
		{
			SendClientMessage(playerid, BRANCO, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp(cmd, "/reconectarnpcs", true) == 0)
	{
		if(pAdmin[playerid] == 5)
		{
			KickNPCs();
			LoadNPCs();

			SendClientMessage(playerid, Verde, "Os npc's foram reconectados com sucesso!");
		}
		return 1;
	}

	if(strcmp(cmd, "/punir", true) == 0)
	{
		if(pAdmin[playerid] > 1)
		{
			new plid, motivo[256];

			if(sscanf(cmdtext, "s[7]us[256]", cmd, plid, motivo))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /punir [id] [motivo]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				aviso[plid] += 1;
				if(aviso[plid] >= 3)
				{
					format(string, sizeof(string), "O(A) jogador(a) %s foi kickado(a) por DUBAYBot. Motivo: 3/3 Punições", GetPlayerNameEx(plid));
					SendClientMessageToAll(Amarelo, string);
					KickLog(string);
					Kick(plid);
				}
				else
				{
					format(string, sizeof(string), "O(A) jogador(a) %s foi punido(a) por %s. Motivo: %s (%d/3)", GetPlayerNameEx(plid), GetPlayerNameEx(playerid), motivo, aviso[plid]);
					SendClientMessageToAll(Azul, string);
				}
			}
			else
			{
				format(string, sizeof(string), " ID %d inválido!", plid);
				SendClientMessage(playerid, Vermelho, string);
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/liberarnick", true) == 0)
	{
		if(pAdmin[playerid] > 2)
		{
			new plid;

			if(sscanf(cmdtext, "s[13]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /liberarnick [id]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				format(file2, sizeof(file2), PASTA_CONTAS, GetPlayerNameEx(plid));
				dini_IntSet(file2, "MN", 1);

				format(string, sizeof(string), "Você liberou o(a) %s para mudar de nick!", GetPlayerNameEx(plid), playerid);
				SendClientMessage(playerid, Blue, string);

				format(string, sizeof(string), "O(A) ADM %s (ID: %d) liberou você para mudar de nick!", GetPlayerNameEx(playerid), playerid);
				SendClientMessage(plid, tcadm, string);

				GameTextForPlayer(plid, "~r~/trocarnick", 8000, 3);
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente!");
			}
		}
		return 1;
	}

	if(strcmp("/ping", cmd, true) == 0)
	{
		if(pAdmin[playerid] > 0)
		{
			new cfg[64];

			if(sscanf(cmdtext, "s[6]s[64]", cmd, cfg))
			{
				SendClientMessage(playerid, 0x00E800FF, "Use: /ping [desativar/ban/kick]");
				SendClientMessage(playerid, 0x00E800FF, "Ex.: /ping ban");
				return 1;
			}
			if(!strcmp(cfg, "desativar", true))
			{
				PingCfg = 0;
				SendClientMessage(playerid, Amarelo, "Limite de ping desativado com sucesso!");
			}
			else if(!strcmp(cfg, "ban", true))
			{
				PingCfg = 1;
				SendClientMessage(playerid, Amarelo, "Agora os players que ultrapassar o limite serão banidos!");
			}
			else if(!strcmp(cfg, "kick", true))
			{
				PingCfg = 2;
				SendClientMessage(playerid, Amarelo, "Agora os players que ultrapassar o limite serão kickados!");
			}
			else SendClientMessage(playerid, 0x00E800FF, "Função não disponível!");
		}
		return 1;
	}

	if(strcmp("/kick", cmd, true) == 0)
	{
		if(pAdmin[playerid] > 1)
		{
			new plid, motivo[64];

			if(sscanf(cmdtext, "s[6]us[64]", cmd, plid, motivo))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /kick [id] [motivo]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				if(IsPlayerNPC(plid))
				{
					SendClientMessage(playerid, Vermelho, "Você não pode fazer isso com um NPC.");
					return 1;
				}
				format(string, sizeof(string), "O(A) jogador(a) %s foi kickado(a) por %s. Motivo: %s", GetPlayerNameEx(plid), GetPlayerNameEx(playerid), motivo);
				SendClientMessageToAll(Amarelo, string);
				KickLog(string);
				Kick(plid);
			}
			else
			{
				format(string, sizeof(string), "Valor inválido, tente novamente!", plid);
				SendClientMessage(playerid, Vermelho, string);
			}
		}
		return 1;
	}

	if(strcmp("/banirnick", cmd, true) == 0)
	{
		if(pAdmin[playerid] == 5)
		{
			new nick[MAX_PLAYER_NAME], motivo[64];

			if(sscanf(cmdtext, "s[11]s[24]s[64]", cmd, nick, motivo))
			{
				SendClientMessage(playerid, Vermelho, "Use: /banirnick [nick] [motivo]");
				return 1;
			}
			VBanNick(playerid, nick, motivo);
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp("/desbanirnick", cmd, true) == 0)
	{
		if(pAdmin[playerid] == 5)
		{
			new nick[MAX_PLAYER_NAME];

			if(sscanf(cmdtext, "s[14]s[24]", cmd, nick))
			{
				SendClientMessage(playerid, Vermelho, "Use: /desbanirnick [nick]");
				return 1;
			}
			VUnBan(playerid, nick);
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp("/ban", cmd, true) == 0)
	{
		if(pAdmin[playerid] > 1)
		{
			new plid, motivo[64];

			if(sscanf(cmdtext, "s[5]us[64]", cmd, plid, motivo))
			{
				SendClientMessage(playerid, Cinza, "Use: /ban [id] [motivo]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				if(IsPlayerNPC(plid))
				{
					SendClientMessage(playerid, Vermelho, "Você não pode fazer isso com um NPC.");
					return 1;
				}
				ClearChatbox(plid, 3);
				VBanID(playerid, plid, motivo);
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente!");
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp("/banip", cmd, true) == 0)
	{
		if(pAdmin[playerid] > 0)
		{
			new ip[32], motivo[64];

			if(sscanf(cmdtext, "s[7]s[32]s[64]", cmd, ip, motivo))
			{
				SendClientMessage(playerid, Cinza, "Use: /banip [ip] [motivo]");
				return 1;
			}
			if(VBIsIP(ip))
			{
				if(strcmp("127.0.0.1", ip, true) == 0)
				{
					SendClientMessage(playerid, Vermelho, "Você não pode fazer isso com este IP.");
					return 1;
				}
				VBanIP(playerid, ip, motivo);
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Endereço IP inválido, tente novamente!");
				SendClientMessage(playerid, Amarelo, "/ip [id]: Para ver o endereço IP de um jogador.");
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp("/desbanir", cmd, true) == 0)
	{
		if(pAdmin[playerid] > 3)
		{
			new info[64];

			if(sscanf(cmdtext, "s[10]s[64]", cmd, info))
			{
				SendClientMessage(playerid, Cinza, "Use: /desbanir [nick/ip]");
				return 1;
			}
			VUnBan(playerid, info);
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp("/infoban", cmd, true) == 0)
	{
		if(pAdmin[playerid] > 3)
		{
			new info[64];

			if(sscanf(cmdtext, "s[9]s[64]", cmd, info))
			{
				SendClientMessage(playerid, Cinza, "Use: /infoban [nick/ip]");
				return 1;
			}
			format(file, sizeof(file), "/Bans/%s.ini", info);
			VBanLoadInfo(playerid, file);
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp("/deletaracc", cmd, true) == 0)
	{
		if(PlayerInfo[playerid][SCON] == true)
		{
			new conta[MAX_PLAYER_NAME];

			if(sscanf(cmdtext, "s[12]s[24]", cmd, conta))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /deletaracc [nick]");
				return 1;
			}

			format(file2, sizeof(file2), PASTA_CONTAS, conta);
			if(dini_Exists(file2))
			{
				dini_Remove(file2);

				format(string, sizeof(string), "O(A) ADM %s excluiu a conta %s.", GetPlayerNameEx(playerid), conta);
				SendClientMessageToAll(tcadm, string);
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Este usuário não existe!");
			}
		}
		return 1;
	}

	if(strcmp("/fakeban", cmd, true) == 0)
	{
		if(pAdmin[playerid] > 3)
		{
			new plid, motivo[64];

			if(sscanf(cmdtext, "s[9]us[64]", cmd, plid, motivo))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /fakeban [id] [motivo]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				format(string, sizeof(string), "Você fakebaniu o(a) jogador(a) %s.", GetPlayerNameEx(plid));
				SendClientMessage(playerid, tcadm, string);

				format(string, sizeof(string), "O(A) jogador(a) %s foi banido(a) por %s. Motivo: %s", GetPlayerNameEx(plid), GetPlayerNameEx(playerid), motivo);
				SendClientMessageToAll(CorBan, string);

				format(string, sizeof(string), "{FFFFFF}-| {33AA33}%s {FFFFFF}deve ter vacilado.", GetPlayerNameEx(plid));
				SendClientMessageToAll(-1, string);

				SendClientMessage(plid, 0xDFDFDFAA, "Server closed the connection.");
			}
			else
			{
				format(string, sizeof(string), "Valor inválido, tente novamente!", plid);
				SendClientMessage(playerid, Vermelho, string);
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/afk", true) == 0)
	{
		if(AFK[playerid] == 0)
		{
			new motivo[64];

			if(sscanf(cmdtext, "s[5]s[64]", cmd, motivo))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /afk [motivo]");
				return 1;
			}
			if(IsPlayerInAnyVehicle(playerid))
			{
				SendClientMessage(playerid, Vermelho, "Você não pode usar este comando dentro de um carro!");
				return 1;
			}
			AFK[playerid] = 1;
			

			SetCameraBehindPlayer(playerid);
			TogglePlayerControllable(playerid, 0);

			TextDrawShowForPlayer(playerid, AfkText);
			TextDrawShowForPlayer(playerid, AfkBackText);

			format(string, sizeof(string), "%s ficou ausente. Motivo: %s ( /afk )", GetPlayerNameEx(playerid), motivo);
			SendClientMessageToAll(0xFF9595AA, string);
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você já está no modo online para poder utilizar esse comando!");
		}
		return 1;
	}

	if(strcmp(cmd, "/ausentes", true) == 0)
	{
		new count = 0, msg[1000];

		SendClientMessage(playerid, 0x008000AA, "~~~~ Jogadores AFK ~~~~");
		for(new i = 0; i < MAX_PLAYERS; i++)
		{
			if(IsPlayerConnected(i) && AFK[i])
			{
				format(msg, sizeof(msg), "%s", GetPlayerNameEx(i));
				SendClientMessage(playerid, 0x0088CAAA, msg);
				count++;
			}
		}
		if(count == 0)
		{
			SendClientMessage(playerid, 0xFF0000AA, "Ninguém está ausente.");
		}
		return 1;
	}

	if(strcmp("/trabalhar", cmd, true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));

		if(dini_Int(file, "Profissao") == Desempregado)
		{
			SendClientMessage(playerid, Vermelho, "Você não tem um emprego e não está trabalhando.");
			SendClientMessage(playerid, Vermelho, "Você pode conseguir um emprego na prefeitura.");
		}
		if(dini_Int(file, "Profissao") == MotoristaP)
		{

		}
		if(dini_Int(file, "Profissao") == Guarda)
		{
			if(GetClosestDelegacia(playerid) == 0)
			{
				SetPlayerInterior(playerid, 0);
				SetPlayerPos(playerid, 308.7138, -1859.3555, 3.0668); // Praia St. M.
			}
			else if(GetClosestDelegacia(playerid) == 1)
			{
				SetPlayerInterior(playerid, 0);
				SetPlayerPos(playerid, -1606.2260, 675.1903, -5.2421); // DPExSF
			}
			else if(GetClosestDelegacia(playerid) == 2)
			{
				SetPlayerInterior(playerid, 0);
				SetPlayerPos(playerid, 2293.6933, 2451.4687, 10.8203); // DPExLV
			}
		}
		if(dini_Int(file, "Profissao") == Policial_R)
		{
			if(GetClosestDelegacia(playerid) == 0)
			{
				SetPlayerInterior(playerid, 0);
				SetPlayerPos(playerid, 1528.0056, -1677.9119, 5.8906); // DPExLS
			}
			else if(GetClosestDelegacia(playerid) == 1)
			{
				SetPlayerInterior(playerid, 0);
				SetPlayerPos(playerid, -1606.2260, 675.1903, -5.2421); // DPExSF
			}
			else if(GetClosestDelegacia(playerid) == 2)
			{
				SetPlayerInterior(playerid, 0);
				SetPlayerPos(playerid, 2293.6933, 2451.4687, 10.8203); // DPExLV
			}
		}
		if(dini_Int(file, "Profissao") == Policial_M)
		{
			if(GetClosestDelegacia(playerid) == 0)
			{
				SetPlayerInterior(playerid, 0);
				SetPlayerPos(playerid, 1528.0056, -1677.9119, 5.8906); // DPExLS
			}
			else if(GetClosestDelegacia(playerid) == 1)
			{
				SetPlayerInterior(playerid, 0);
				SetPlayerPos(playerid, -1606.2260, 675.1903, -5.2421); // DPExSF
			}
			else if(GetClosestDelegacia(playerid) == 2)
			{
				SetPlayerInterior(playerid, 0);
				SetPlayerPos(playerid, 2293.6933, 2451.4687, 10.8203); // DPExLV
			}
		}
		if(dini_Int(file, "Profissao") == Policial_C)
		{
			if(GetClosestDelegacia(playerid) == 0)
			{
				SetPlayerInterior(playerid, 0);
				SetPlayerPos(playerid, 1528.0056, -1677.9119, 5.8906); // DPExLS
			}
			else if(GetClosestDelegacia(playerid) == 1)
			{
				SetPlayerInterior(playerid, 0);
				SetPlayerPos(playerid, -1606.2260, 675.1903, -5.2421); // DPExSF
			}
			else if(GetClosestDelegacia(playerid) == 2)
			{
				SetPlayerInterior(playerid, 0);
				SetPlayerPos(playerid, 2293.6933, 2451.4687, 10.8203); // DPExLV
			}
		}
		if(dini_Int(file, "Profissao") == Policial_F)
		{
			if(GetClosestDelegacia(playerid) == 0)
			{
				SetPlayerInterior(playerid, 0);
				SetPlayerPos(playerid, 1528.0056, -1677.9119, 5.8906); // DPExLS
			}
			else if(GetClosestDelegacia(playerid) == 1)
			{
				SetPlayerInterior(playerid, 0);
				SetPlayerPos(playerid, -1606.2260, 675.1903, -5.2421); // DPExSF
			}
			else if(GetClosestDelegacia(playerid) == 2)
			{
				SetPlayerInterior(playerid, 0);
				SetPlayerPos(playerid, 2293.6933, 2451.4687, 10.8203); // DPExLV
			}
		}
		if(dini_Int(file, "Profissao") == Delegado)
		{
			if(GetClosestDelegacia(playerid) == 0)
			{
				SetPlayerInterior(playerid, 0);
				SetPlayerPos(playerid, 1528.0056, -1677.9119, 5.8906); // DPExLS
			}
			else if(GetClosestDelegacia(playerid) == 1)
			{
				SetPlayerInterior(playerid, 0);
				SetPlayerPos(playerid, -1606.2260, 675.1903, -5.2421); // DPExSF
			}
			else if(GetClosestDelegacia(playerid) == 2)
			{
				SetPlayerInterior(playerid, 0);
				SetPlayerPos(playerid, 2293.6933, 2451.4687, 10.8203); // DPExLV
			}
		}
		if(dini_Int(file, "Profissao") == Bope)
		{
			if(GetClosestDelegacia(playerid) == 0)
			{
				SetPlayerInterior(playerid, 0);
				SetPlayerPos(playerid, 1528.0056, -1677.9119, 5.8906); // DPExLS
			}
			else if(GetClosestDelegacia(playerid) == 1)
			{
				SetPlayerInterior(playerid, 0);
				SetPlayerPos(playerid, -1606.2260, 675.1903, -5.2421); // DPExSF
			}
			else if(GetClosestDelegacia(playerid) == 2)
			{
				SetPlayerInterior(playerid, 0);
				SetPlayerPos(playerid, 2293.6933, 2451.4687, 10.8203); // DPExLV
			}
		}
		if(dini_Int(file, "Profissao") == Swat)
		{
			SetPlayerInterior(playerid, 0);
			SetPlayerPos(playerid, 1182.0980, -2036.8110, 69.0078); // SWAT BASE LS
		}
		if(dini_Int(file, "Profissao") == Narcoticos)
		{
			if(GetClosestDelegacia(playerid) == 0)
			{
				SetPlayerInterior(playerid, 0);
				SetPlayerPos(playerid, 1528.0056, -1677.9119, 5.8906); // DPExLS
			}
			else if(GetClosestDelegacia(playerid) == 1)
			{
				SetPlayerInterior(playerid, 0);
				SetPlayerPos(playerid, -1606.2260, 675.1903, -5.2421); // DPExSF
			}
			else if(GetClosestDelegacia(playerid) == 2)
			{
				SetPlayerInterior(playerid, 0);
				SetPlayerPos(playerid, 2293.6933, 2451.4687, 10.8203); // DPExLV
			}
		}
		if(dini_Int(file, "Profissao") == Traficante)
		{
			SetPlayerInterior(playerid, 0);
			SetPlayerPos(playerid, 2154.8842, -1071.1025, 39.2741);
		}
		if(dini_Int(file, "Profissao") == Assasino)
		{

		}
		if(dini_Int(file, "Profissao") == Terrorista)
		{

		}
		if(dini_Int(file, "Profissao") == Sequestrador)
		{

		}
		if(dini_Int(file, "Profissao") == AssasinoProfissional)
		{

		}
		if(dini_Int(file, "Profissao") == Jornalista)
		{

		}
		if(dini_Int(file, "Profissao") == Fotografo)
		{

		}
		if(dini_Int(file, "Profissao") == Reporter)
		{

		}
		if(dini_Int(file, "Profissao") == Ancora)
		{

		}
		if(dini_Int(file, "Profissao") == Meteorologista)
		{

		}
		if(dini_Int(file, "Profissao") == Mecanico)
		{

		}
		if(dini_Int(file, "Profissao") == Rapper)
		{

		}
		if(dini_Int(file, "Profissao") == VendedorSkin)
		{

		}
		if(dini_Int(file, "Profissao") == VendedorCarro)
		{

		}
		if(dini_Int(file, "Profissao") == Frentista)
		{

		}
		if(dini_Int(file, "Profissao") == Taxista)
		{
			SetPlayerInterior(playerid, 0);
			SetPlayerPos(playerid, 1758.1934, -1860.5705, 13.5785);
		}
		if(dini_Int(file, "Profissao") == FBI)
		{
			if(GetClosestDelegacia(playerid) == 0)
			{
				SetPlayerInterior(playerid, 0);
				SetPlayerPos(playerid, 1528.0056, -1677.9119, 5.8906); // DPExLS
			}
			else if(GetClosestDelegacia(playerid) == 1)
			{
				SetPlayerInterior(playerid, 0);
				SetPlayerPos(playerid, -1606.2260, 675.1903, -5.2421); // DPExSF
			}
			else if(GetClosestDelegacia(playerid) == 2)
			{
				SetPlayerInterior(playerid, 0);
				SetPlayerPos(playerid, 2293.6933, 2451.4687, 10.8203); // DPExLV
			}
		}
		if(dini_Int(file, "Profissao") == Interpol)
		{
			if(GetClosestDelegacia(playerid) == 0)
			{
				SetPlayerInterior(playerid, 0);
				SetPlayerPos(playerid, 1528.0056, -1677.9119, 5.8906); // DPExLS
			}
			else if(GetClosestDelegacia(playerid) == 1)
			{
				SetPlayerInterior(playerid, 0);
				SetPlayerPos(playerid, -1606.2260, 675.1903, -5.2421); // DPExSF
			}
			else if(GetClosestDelegacia(playerid) == 2)
			{
				SetPlayerInterior(playerid, 0);
				SetPlayerPos(playerid, 2293.6933, 2451.4687, 10.8203); // DPExLV
			}
		}
		if(dini_Int(file, "Profissao") == Aposentado)
		{

		}
		if(dini_Int(file, "Profissao") == Advogado)
		{

		}
		if(dini_Int(file, "Profissao") == Prostituta)
		{

		}
		if(dini_Int(file, "Profissao") == Promoter)
		{

		}
		if(dini_Int(file, "Profissao") == SegVila)
		{

		}
		if(dini_Int(file, "Profissao") == Assaltante)
		{

		}
		if(dini_Int(file, "Profissao") == Bibliotecario)
		{

		}
		if(dini_Int(file, "Profissao") == Religioso)
		{

		}
		if(dini_Int(file, "Profissao") == Prefeito)
		{

		}
		if(dini_Int(file, "Profissao") == Presidente)
		{

		}
		if(dini_Int(file, "Profissao") == TraficanteD)
		{

		}
		if(dini_Int(file, "Profissao") == Mendigo)
		{

		}
		if(dini_Int(file, "Profissao") == LSPD)
		{
			if(GetClosestDelegacia(playerid) == 0)
			{
				SetPlayerInterior(playerid, 0);
				SetPlayerPos(playerid, 1528.0056, -1677.9119, 5.8906); // DPExLS
			}
			else if(GetClosestDelegacia(playerid) == 1)
			{
				SetPlayerInterior(playerid, 0);
				SetPlayerPos(playerid, -1606.2260, 675.1903, -5.2421); // DPExSF
			}
			else if(GetClosestDelegacia(playerid) == 2)
			{
				SetPlayerInterior(playerid, 0);
				SetPlayerPos(playerid, 2293.6933, 2451.4687, 10.8203); // DPExLV
			}
		}
		if(dini_Int(file, "Profissao") == YKZ)
		{

		}
		if(dini_Int(file, "Profissao") == MRN)
		{

		}
		if(dini_Int(file, "Profissao") == Empregada)
		{

		}
		if(dini_Int(file, "Profissao") == Pedreiro)
		{

		}
		if(dini_Int(file, "Profissao") == Gari)
		{

		}
		if(dini_Int(file, "Profissao") == Lixeiro)
		{

		}
		if(dini_Int(file, "Profissao") == Temac)
		{

		}
		if(dini_Int(file, "Profissao") == Correio)
		{

		}
		if(dini_Int(file, "Profissao") == Estudante)
		{

		}
		if(dini_Int(file, "Profissao") == Flanelinha)
		{

		}
		if(dini_Int(file, "Profissao") == Cantor)
		{

		}
		if(dini_Int(file, "Profissao") == Poeta)
		{

		}
		if(dini_Int(file, "Profissao") == Mafia)
		{

		}
		if(dini_Int(file, "Profissao") == Drifter)
		{

		}
		if(dini_Int(file, "Profissao") == Professor)
		{

		}
		if(dini_Int(file, "Profissao") == Empregador)
		{

		}
		if(dini_Int(file, "Profissao") == AtiradorElite)
		{

		}
		if(dini_Int(file, "Profissao") == Ninja)
		{

		}
		if(dini_Int(file, "Profissao") == Maquinista)
		{
			SetPlayerInterior(playerid, 0);
			SetPlayerPos(playerid, 1697.3809, -1949.3073, 14.1171);
		}
		if(dini_Int(file, "Profissao") == Caminhoneiro)
		{
			if(GetClosestCargas(playerid) == 0)
			{
				ShowPlayerLocationFromGPS(playerid, "Polo Industrial", 2413.9599, -2089.2497, 13.4353); // LS
				ShowPlayerDialog(playerid, playersimp, DIALOG_STYLE_MSGBOX, "Caminhoneiro", "\t{00FF00}GPS Ativado!\n{FFFFFF}Agora siga a seta em cima de seu carro para chegar até o polo industrial mais próximo.", "OK", "");
			}
			else if(GetClosestCargas(playerid) == 1)
			{
				ShowPlayerLocationFromGPS(playerid, "Polo Industrial", -1636.4776, 57.9578, 3.2608); // SF
				ShowPlayerDialog(playerid, playersimp, DIALOG_STYLE_MSGBOX, "Caminhoneiro", "\t{00FF00}GPS Ativado!\n{FFFFFF}Agora siga a seta em cima de seu carro para chegar até o polo industrial mais próximo.", "OK", "");
			}
			else if(GetClosestCargas(playerid) == 2)
			{
				ShowPlayerLocationFromGPS(playerid, "Polo Industrial", -493.0995, -541.6450, 25.5234); // SF
				ShowPlayerDialog(playerid, playersimp, DIALOG_STYLE_MSGBOX, "Caminhoneiro", "\t{00FF00}GPS Ativado!\n{FFFFFF}Agora siga a seta em cima de seu carro para chegar até o polo industrial mais próximo.", "OK", "");
			}
			else if(GetClosestCargas(playerid) == 3)
			{
				ShowPlayerLocationFromGPS(playerid, "Polo Industrial", 1387.3312, 2302.7683, 10.8127); // LV
				ShowPlayerDialog(playerid, playersimp, DIALOG_STYLE_MSGBOX, "Caminhoneiro", "\t{00FF00}GPS Ativado!\n{FFFFFF}Agora siga a seta em cima de seu carro para chegar até o polo industrial mais próximo.", "OK", "");
			}
			else if(GetClosestCargas(playerid) == 4)
			{
				ShowPlayerLocationFromGPS(playerid, "Polo Industrial", 1007.4269, 2133.3259, 10.6718); // LV
				ShowPlayerDialog(playerid, playersimp, DIALOG_STYLE_MSGBOX, "Caminhoneiro", "\t{00FF00}GPS Ativado!\n{FFFFFF}Agora siga a seta em cima de seu carro para chegar até o polo industrial mais próximo.", "OK", "");
			}
			else if(GetClosestCargas(playerid) == 5)
			{
				ShowPlayerLocationFromGPS(playerid, "Polo Industrial", 2347.7038, 2713.3149, 10.8818); // LV
				ShowPlayerDialog(playerid, playersimp, DIALOG_STYLE_MSGBOX, "Caminhoneiro", "\t{00FF00}GPS Ativado!\n{FFFFFF}Agora siga a seta em cima de seu carro para chegar até o polo industrial mais próximo.", "OK", "");
			}
		}
		if(dini_Int(file, "Profissao") == Petroleiro)
		{

		}
		if(dini_Int(file, "Profissao") == MOD)
		{

		}
		PlayerPlaySound(playerid, 1057, 0, 0, 0);
		return 1;
	}

	if(strcmp("/pizzaboy", cmd, true) == 0)
	{
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~ Pizza-boy ~~~~~~~~~~~~~~~~~~~~~");
		SendClientMessage(playerid, Branco, "Entrega de pizzas!");
		SendClientMessage(playerid, Branco, "Area de pegar pizzas: simbolizado por o ícones de ''Hamburgers'', ''Pizzas'' ou ''Frangos'' por Los Santos.");
		SendClientMessage(playerid, Branco, "Area de entregar pizzas: simbolizado pelo ícone de ''Garfo e Faca'' na entrada de Los Santos.");
		SendClientMessage(playerid, Branco, "Valor recebido: $500 - Por cada entrega.");
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~ Pizza-boy ~~~~~~~~~~~~~~~~~~~~~");
		return 1;
	}

	if(strcmp("/pescador", cmd, true) == 0)
	{
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~ Pescador ~~~~~~~~~~~~~~~~~~~~~");
		SendClientMessage(playerid, Branco, "Pesque em alto mar e venda sua pesca!");
		SendClientMessage(playerid, Branco, "Area de pesca: simbolizado pelo ícone de uma ''Ancora'' em Santa Maria Beach. (Los Santos)");
		SendClientMessage(playerid, Branco, "Area de venda de pesca: simbolizado pelo ícone ''Vermelho'' na areia da Praia. (Los Santos)");
		SendClientMessage(playerid, Branco, "Valor recebido: $130 - por cada peixe pescado.");
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~ Pescador ~~~~~~~~~~~~~~~~~~~~~");
		return 1;
	}

	if(strcmp("/lenhador", cmd, true) == 0)
	{
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~ Lenhador ~~~~~~~~~~~~~~~~~~~~~");
		SendClientMessage(playerid, Branco, "Corte e venda de madeiras ilegais!");
		SendClientMessage(playerid, Branco, "Area de corte: simbolizado por o ícone de um ''C-'' no mapa na entrada de Los Santos.");
		SendClientMessage(playerid, Branco, "Area de venda de madeira: simbolizado por o ícone de um ''Boneco Verde'' na entrada de Los Santos.");
		SendClientMessage(playerid, Branco, "Valor recebido: $80 - Por cada madeira.");
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~ Lenhador ~~~~~~~~~~~~~~~~~~~~~");
		return 1;
	}

	if(strcmp("/pagamento", cmd, true) == 0)
	{
		if(pAdmin[playerid] > 3)
		{
			Up();

			format(string, sizeof(string), "O(A) ADM %s adiantou o salário de todos!", GetPlayerNameEx(playerid));
			SendClientMessageToAll(BLUEWHITE, string);
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp(cmd, "/cp", true) == 0)
	{
		new texto[128];

		if(sscanf(cmdtext, "s[4]s[128]", cmd, texto))
		{
			SendClientMessage(playerid, Vermelho, "/cp [texto]");
		}
		else
		{
			format(string, sizeof(string), "(Chat Profissão) %s: %s", GetPlayerNameEx(playerid), texto);
			Chatp(GetPlayerColor(playerid), string, 1, playerid);
		}
		return 1;
	}

	if(strcmp(cmd, "/virar", true) == 0)
	{
		if(pAdmin[playerid] > 0 || IsPlayerVIP(playerid))
		{
			if(IsPlayerInAnyVehicle(playerid))
			{
				FlipCar(GetPlayerVehicleID(playerid));
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/virarp", true) == 0)
	{
		if(pAdmin[playerid] > 0)
		{
			new plid;

			if(sscanf(cmdtext, "s[8]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /virarp [id]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				if(IsPlayerInAnyVehicle(plid))
				{
					FlipCar(GetPlayerVehicleID(plid));

					format(string, sizeof(string), "O(A) ADM %s (%d) virou seu veículo!", GetPlayerNameEx(playerid), playerid);
					SendClientMessage(plid, tcadm, string);

					SendClientMessage(playerid, Verde, "Comando efetuado com sucesso!");
				}
			}
		}
		return 1;
	}

	if(strcmp("/ip", cmd, true) == 0)
	{
		if(pAdmin[playerid] > 2)
		{
			new plid;

			if(sscanf(cmdtext, "s[4]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "/ip [id]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				format(string, sizeof(string), "-| %s (ID: %d) IP: %s |-", GetPlayerNameEx(plid), plid, GetPlayerIPEx(plid));
				SendClientMessage(playerid, 0x0080FFAA, string);
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente!");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/rv", true) == 0)
	{
		if(pAdmin[playerid] > 3)
		{
			for(new v; v < MAX_VEHICLES; v++)
			{
				SetVehicleToRespawn(v);
			}
			format(string, sizeof(string), "O(A) ADM %s (%d) respawnou todos os veículos.", GetPlayerNameEx(playerid), playerid);
			SendClientMessageToAll(tcadm, string);

			SendClientMessage(playerid, Verde, "Veículos respawnados!");
			return 1;
		}
	}

	if(strcmp(cmd, "/rvu", true) == 0)
	{
		if(pAdmin[playerid] > 3)
		{
			for(new v = 0; v < MAX_VEHICLES; v++)
			{
				if(!IsVehicleOccupied(v)) SetVehicleToRespawn(v);
			}
			format(string, sizeof(string), "O(A) ADM %s (%d) respawnou todos os veículos desocupados.", GetPlayerNameEx(playerid), playerid);
			SendClientMessageToAll(tcadm, string);

			SendClientMessage(playerid, Verde, "Veículos desocupados respawnados!");
		}
		return 1;
	}

	if(strcmp(cmd, "/rvc", true) == 0)
	{
		if(pAdmin[playerid] > 3)
		{
			for(new carro = 0; carro < MAX_CONCES; carro++)
			{
				format(file, sizeof(file), PASTA_CONCE, carro);
				if(dini_Exists(file))
				{
					DestroyVehicle(dini_Int(file, "Id"));
					CriarVeiculo3(carro, dini_Int(file, "Modelo"), dini_Float(file, "CordX"), dini_Float(file, "CordY"), dini_Float(file, "CordZ"), dini_Float(file, "Angulo"), dini_Int(file, "Cor1"), dini_Int(file, "Cor2"));

					if(strcmp(dini_Get(file, "Dono"), "Ninguem", true) == 0)
					{
						if(!(dini_Int(file, "TDono") == 3))
						{
							dini_IntSet(file, "TDono", 0);
						}
					}
				}
			}
			format(string, sizeof(string), "O(A) ADM %s (%d) respawnou os veículos da Conce.", GetPlayerNameEx(playerid), playerid);
			SendClientMessageToAll(tcadm, string);

			SendClientMessage(playerid, Verde, "Veículos da Conce respawnados!");
		}
		return 1;
	}

	if(strcmp(cmd, "/objstatus", true) == 0)
	{
		format(string, sizeof(string), "Existe %d objetos ao total no servidor.", CountDynamicObjects());
		SendClientMessage(playerid, Amarelo, string);
		return 1;
	}

	if(strcmp(cmd, "/vehstatus", true) == 0)
	{
		format(string, sizeof(string), "Existe %d veículos na Conce do servidor.", proximocarro);
		SendClientMessage(playerid, Amarelo, string);
		return 1;
	}

	if(strcmp(cmd, "/rmov", true) == 0)
	{
		if(pAdmin[playerid] > 4)
		{
			if(EventoCriado == 1)
			{
				SendClientMessage(playerid, Vermelho, "Antes de fazer isso feche ou cancele o evento.");
				return 1;
			}

			for(new i = 0; i < MAX_PLAYERS; i++)
			{
				if(IsPlayerConnected(i))
				{
					PlayerTextDrawHide(i, PlayerInfo[i][AreaName]);

					#if defined gText1User
					PlayerTextDrawHide(i, PlayerInfo[i][gText1]);
					#endif

					#if defined gText2User
					PlayerTextDrawHide(i, PlayerInfo[i][gText2]);
					#endif

					TextDrawHideForPlayer(i, Relogio);

					if(AFK[playerid] == 1)
					{
						TextDrawHideForPlayer(playerid, AfkText);
						TextDrawHideForPlayer(playerid, AfkBackText);
					}
					DeletePlayerTextDraws(i);

					GangZoneHideForPlayer(i, GangZonesFix[1]);
					GangZoneHideForPlayer(i, GangZonesFix[2]);
					GangZoneHideForPlayer(i, GangZonesFix[3]);
					GangZoneHideForPlayer(i, GangZonesFix[4]);
					GangZoneHideForPlayer(i, GangZonesFix[5]);
					GangZoneHideForPlayer(i, GangZonesFix[6]);
					GangZoneHideForPlayer(i, GangZonesFix[7]);
					GangZoneHideForPlayer(i, GangZonesFix[8]);
					GangZoneHideForPlayer(i, GangZonesFix[9]);
					SpawnPlayer(i);
				}
			}

			DeleteTextDraws();
			Destroy3DTextsFix();
			DestroyCheckpointsFix();
			DestroyGangZonesFix();
			DestroyMapIconsFix();
			DestroyPickupsFix();
			DestroyObjectsFix();
			DestroyVehiclesFix();
			UnloadFilesIniItens();
			UnloadAllDynamicObjects();
			UnloadAllStaticVehicles();

			SetTimerEx("RespawnSystem", 1000, false, "e", playerid);

			ClearChatbox(playerid, 3);
			SendClientMessage(playerid, Azul, "Seu pedido foi processado, aguarde...");
			ClearChatbox(playerid, 3);
		}
		return 1;
	}

	if(strcmp(cmd, "/deletcar", true) == 0)
	{
		if(pAdmin[playerid] > 3)
		{
			Loop(o, sizeof(VehiclesFix))
			{
				if(GetPlayerVehicleID(playerid) == VehiclesFix[o])
				{
					SendClientMessage(playerid, Vermelho, "Este carro não pode ser deletado!");
					return 1;
				}
			}
			for(new carro = 0; carro < MAX_CONCES; carro++)
			{
				format(string, sizeof(string), PASTA_CONCE, carro);
				if(GetPlayerVehicleID(playerid) == dini_Int(string, "Id"))
				{
					ShowPlayerDialog(playerid, deletcarconce, DIALOG_STYLE_MSGBOX, "Deletando Veículo", "{FF0000}Este veículo é da Conce, quer mesmo deletar?", "Sim", "Não");
					return 1;
				}
			}
			DestroyVehicle(GetPlayerVehicleID(playerid));
			SendClientMessage(playerid, 0x0080FFAA, "Veículo deletado com sucesso!");
			return 1;
		}
	}

	if(strcmp(cmd, "/respawncar", true) == 0)
	{
		if(pAdmin[playerid] > 0)
		{
			SetVehicleToRespawn(GetPlayerVehicleID(playerid));
			SendClientMessage(playerid, 0x0080FFAA, "Veículo respawnado com sucesso!");
		}
		return 1;
	}

	if(strcmp("/explodir", cmd, true) == 0)
	{
		if(pAdmin[playerid] > 4)
		{
			new plid, Float:X, Float:Y, Float:Z;

			if(sscanf(cmdtext, "s[10]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /explodir [id]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				GetPlayerPos(plid, X, Y, Z);
				SetPlayerHealth(plid, 95.0);
				CreateExplosion(X, Y, Z-3, 10, 100);

				format(string, sizeof(string), "O(A) ADM %s (%d) explodiu você!", GetPlayerNameEx(playerid), playerid);
				SendClientMessage(plid, tcadm, string);

				SendClientMessage(playerid, Verde, "Comando efetuado com sucesso!");
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente!");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/recuperar", true) == 0)
	{
		if(pAdmin[playerid] > 1)
		{
			SendClientMessage(playerid, COLOR_GREEN, "Vida e coletes recuperados!");
			for(new i = 0; i < MAX_PLAYERS; i++)
			{
				if(IsPlayerConnected(i))
				{
					SetPlayerArmour(i, 100.0);
					SetPlayerHealth(i, 100.0);
				}
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/godmod", true) == 0)
	{
		if(pAdmin[playerid] > 4)
		{
			gmod[playerid] = 1;
			SetPlayerHealth(playerid, 999999.0);
			SendClientMessage(playerid, COLOR_GREEN, "Agora você não morrerá fácil.");
		}
		return 1;
	}

	if(strcmp(cmd, "/godcar", true) == 0)
	{
		if(pAdmin[playerid] > 4)
		{
			if(!IsPlayerInAnyVehicle(playerid))
			{
				SendClientMessage(playerid, 0xE60000FF, "Você precisa estar em um veículo!");
				return 1;
			}
			vmod[playerid] = 1;
			RepairVehicle(GetPlayerVehicleID(playerid));
			SendClientMessage(playerid, COLOR_GREEN, "Agora vai ser difícil seu veículo explodir.");
		}
		return 1;
	}

	if(strcmp(cmd, "/lc", true) == 0)
	{
		if(pAdmin[playerid] > 0)
		{
			for(new i = 0; i < MAX_PLAYERS; i++)
			{
				if(IsPlayerConnected(i))
				{
					ClearChatbox(i, 30);
				}
			}
			GameTextForAll("~w~Chat ~p~Limpo", 1000, 1);
		}
		return 1;
	}

	if(strcmp(cmd, "/jetpack", true) == 0)
	{
		if(pAdmin[playerid] > 1)
		{
			SetPlayerSpecialAction(playerid, 2);
			SendClientMessage(playerid, COLOR_GREEN, "(SERVER) Jetpack criado!");
		}
		return 1;
	}

	if(strcmp("/vida", cmd, true) == 0)
	{
		if(pAdmin[playerid] > 4)
		{
			new plid, Float:vida;

			if(sscanf(cmdtext, "s[6]uf", cmd, plid, vida))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /vida [id] [quantidade]");
				return 1;
			}
			if(vida < 1.0 || vida > 100.0)
			{
				SendClientMessage(playerid, Vermelho, "A quantidade máxima é de 100.0");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				SetPlayerHealth(plid, vida);

				format(string, sizeof(string), "O(A) ADM %s (%d) alterou sua vida para: %f", GetPlayerNameEx(playerid), playerid, vida);
				SendClientMessage(plid, tcadm, string);

				SendClientMessage(playerid, Verde, "Comando efetuado com sucesso!");
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Jogador não conectado!");
			}
		}
		return 1;
	}

	if(strcmp("/colete", cmd, true) == 0)
	{
		if(pAdmin[playerid] > 4)
		{
			new plid, Float:colete;

			if(sscanf(cmdtext, "s[8]uf", cmd, plid, colete))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /colete [id] [quantidade]");
				return 1;
			}
			if(colete < 0.0 || colete > 100.0)
			{
				SendClientMessage(playerid, Vermelho, "A quantidade máxima é de 100.0");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				SetPlayerArmour(plid, colete);

				format(string, sizeof(string), "%s (%d) alterou seu colete para: %f", GetPlayerNameEx(playerid), playerid, colete);
				SendClientMessage(plid,tcadm, string);

				SendClientMessage(playerid, Verde, "Comando efetuado com sucesso!");
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Jogador não conectado!");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/ir", true) == 0)
	{
		if(pAdmin[playerid] > 4)
		{
			new plid, Float:X, Float:Y, Float:Z;

			if(sscanf(cmdtext, "s[4]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /ir [id]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				GetPlayerPos(plid, X, Y, Z);
				SetPlayerInterior(playerid, GetPlayerInterior(plid));

				if(IsPlayerInAnyVehicle(playerid))
				{
					SetVehiclePos(GetPlayerVehicleID(playerid), X+2, Y+2, Z);
					PutPlayerInVehicle(playerid, GetPlayerVehicleID(playerid), 0);
				}
				else
				{
					SetPlayerPos(playerid, X+2, Y+2, Z);

					TogglePlayerControllable(playerid, false);
					SetTimerEx("DestogglePlayerControllable", 1000, 0, "d", playerid);
				}
				format(string, sizeof(string), "O(A) ADM %s (%d) foi até sua posição para ajuda-lo(a)!", GetPlayerNameEx(playerid), playerid);
				SendClientMessage(plid, tcadm, string);
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente!");
			}
		}
		return 1;
	}

	if(strcmp("/tapa", cmd, true) == 0)
	{
		if(pAdmin[playerid] > 4)
		{
			new plid, Float:X, Float:Y, Float:Z;

			if(sscanf(cmdtext, "s[6]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /tapa [id]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				GetPlayerPos(plid, X, Y, Z);
				SetPlayerInterior(plid, GetPlayerInterior(plid));
				SetPlayerPos(plid, X, Y, Z +50);

				format(string, sizeof(string), "O(A) ADM %s (%d) deu um tapa em você!", GetPlayerNameEx(playerid), playerid);
				SendClientMessage(plid,tcadm, string);

				SendClientMessage(playerid, Verde, "Comando efetuado com sucesso!");
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente!");
			}
		}
		return 1;
	}

	if(strcmp("/cercar", cmd, true) == 0)
	{
		if(pAdmin[playerid] > 0)
		{
			new plid, Float:px, Float:py, Float:pz;

			if(sscanf(cmdtext, "s[8]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /cercar [id]");
				return 1;
			}
			if(plid == playerid)
			{
				SendClientMessage(playerid, Vermelho, "Você não pode fazer isso com si mesmo.");
				return 1 ;
			}
			if(cercado[plid] == 1)
			{
				SendClientMessage(playerid, Vermelho, "O(A) jogador(a) já está cercado(a).");
				return 1 ;
			}
			if(IsPlayerConnected(plid))
			{
				CagePlayer(plid);
				GetPlayerPos(plid, px, py, pz);
				SetPlayerPos(plid, px, py, pz+7);
				cercado[plid] = 1;

				TogglePlayerControllable(plid, false);
				SetTimerEx("DestogglePlayerControllable", 1000, 0, "d", plid);

				format(string, sizeof(string), "O(A) ADM %s (%d) cercou você!", GetPlayerNameEx(playerid), playerid);
				SendClientMessage(plid, tcadm, string);

				SendClientMessage(playerid, Verde, "Comando efetuado com sucesso!");
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente!");
			}
		}
		return 1;
	}

	if(strcmp("/descercar", cmd, true) == 0)
	{
		if(pAdmin[playerid] > 0)
		{
			new plid, Float:px, Float:py, Float:pz;

			if(sscanf(cmdtext, "s[11]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /descercar [id]");
				return 1;
			}
			if(plid == playerid)
			{
				SendClientMessage(playerid, Vermelho, "Você não pode fazer isto com si mesmo.");
				return 1 ;
			}
			if(cercado[plid] == 0)
			{
				SendClientMessage(playerid, Vermelho, "O(A) jogador(a) não está cercado(a).");
				return 1 ;
			}
			if(IsPlayerConnected(plid))
			{
				UncagePlayer(plid);
				GetPlayerPos(plid, px, py, pz);
				SetPlayerPos(plid, px, py, pz-4);
				cercado[plid] = 0;

				format(string, sizeof(string), "O(A) ADM %s (%d) descercou você!", GetPlayerNameEx(playerid), playerid);
				SendClientMessage(plid, tcadm, string);

				SendClientMessage(playerid, Verde, "Comando efetuado com sucesso!");
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente!");
			}
		}
		return 1;
	}

	if(strcmp("/congelar", cmd, true) == 0)
	{
		if(pAdmin[playerid] > 0)
		{
			new plid;

			if(sscanf(cmdtext, "s[10]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /congelar [id]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				TogglePlayerControllable(plid, 0);

				format(string, sizeof(string), "O(A) ADM %s (%d) congelou você!", GetPlayerNameEx(playerid), playerid);
				SendClientMessage(plid, tcadm, string);

				SendClientMessage(playerid, Verde, "Comando efetuado com sucesso!");
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente!");
			}
		}
		return 1;
	}

	if(strcmp("/descongelar", cmd, true) == 0)
	{
		if(pAdmin[playerid] > 1)
		{
			new plid;

			if(sscanf(cmdtext, "s[13]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /descongelar [id]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				TogglePlayerControllable(plid, 1);

				format(string, sizeof(string), "O(A) ADM %s (%d) descongelou você!", GetPlayerNameEx(playerid), playerid);
				SendClientMessage(plid, tcadm, string);

				SendClientMessage(playerid, Verde, "Comando efetuado com sucesso!");
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente!");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/desarmar", true) == 0)
	{
		if(pAdmin[playerid] > 1)
		{
			new plid;

			if(sscanf(cmdtext, "s[10]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /desarmar [id]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				ResetPlayerWeapons(plid);

				format(string, sizeof(string), "O(A) ADM %s (%d) te desarmou!", GetPlayerNameEx(playerid), playerid);
				SendClientMessage(plid, tcadm, string);

				SendClientMessage(playerid, Verde, "Comando efetuado com sucesso!");
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente!");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/trazer", true) == 0)
	{
		if(pAdmin[playerid] > 1)
		{
			new plid, VehicleID, Float:X, Float:Y, Float:Z;

			if(sscanf(cmdtext, "s[8]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /trazer [id]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				SetPlayerInterior(plid, GetPlayerInterior(playerid));
				GetPlayerPos(playerid, X, Y, Z);

				if(IsPlayerInAnyVehicle(plid))
				{
					VehicleID = GetPlayerVehicleID(plid);
					SetVehiclePos(VehicleID, X+2, Y+2, Z);
					PutPlayerInVehicle(plid, VehicleID, 0);
				}
				else
				{
					SetPlayerPos(plid, X+2, Y+2, Z);

					TogglePlayerControllable(plid, false);
					SetTimerEx("DestogglePlayerControllable", 1000, 0, "d", plid);
				}
				format(string, sizeof(string), "%s (ID: %d) trouxe você até sua posição!", GetPlayerNameEx(playerid), playerid);
				SendClientMessage(plid, tcadm, string);

				format(string, sizeof(string), "Você trouxe %s (ID: %d) até sua posição!", GetPlayerNameEx(plid), plid);
				SendClientMessage(playerid, tcadm, string);
			}
			else
			{
				SendClientMessage(playerid, 0xFF0000AA, "Valor inválido, tente novamente!");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/calar", true) == 0)
	{
		if(pAdmin[playerid] > 1)
		{
			new plid;

			if(sscanf(cmdtext, "s[7]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /calar [id]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				Mute[plid] = 1;

				format(string, sizeof(string), "O(A) ADM %s (%d) pregou um durex na boca do(a): %s (%d)!", GetPlayerNameEx(playerid), playerid, GetPlayerNameEx(plid), plid);
				SendClientMessageToAll(tcadm, string);
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente!");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/descalar", true) == 0)
	{
		if(pAdmin[playerid] > 1)
		{
			new plid;

			if(sscanf(cmdtext, "s[10]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /descalar [id]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				Mute[plid] = 0;

				format(string, sizeof(string), "O(A) ADM %s (%d) tirou o durex da boca do(a): %s (%d)!", GetPlayerNameEx(playerid), playerid, GetPlayerNameEx(plid), plid);
				SendClientMessageToAll(tcadm, string);
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente!");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/resetarflood", true) == 0)
	{
		if(pAdmin[playerid] > 1)
		{
			new plid;

			if(sscanf(cmdtext, "s[14]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /resetarflood [id]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				FloodAlert[plid] = 0;

				format(string, sizeof(string), "O(A) ADM %s (%d) resetou seu flood!",GetPlayerNameEx(playerid), playerid);
				SendClientMessage(plid, tcadm, string);
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente!");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/carona", true) == 0)
	{
		if(pAdmin[playerid] > 0)
		{
			new plid, VehicleID;

			if(sscanf(cmdtext, "s[8]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /carona [id]");
				return 1;
			}
			if(plid == playerid)
			{
				SendClientMessage(playerid, Vermelho, "Você não pode pegar uma carona com você mesmo.");
				return 1;
			}
			if(!IsPlayerConnected(plid))
			{
				SendClientMessage(playerid, Vermelho, "O(A) jogador(a) não está conectado.");
			}
			else
			{
				if(IsPlayerInAnyVehicle(plid))
				{
					VehicleID = GetPlayerVehicleID(plid);
					PutPlayerInVehicle(playerid, VehicleID, 1);

					format(string, sizeof(string), "O(A) ADM %s (%d) entrou no seu carro!", GetPlayerNameEx(playerid), playerid);
					SendClientMessage(plid, tcadm, string);

					SendClientMessage(playerid, Verde, "Comando efetuado com sucesso!");
				}
				else
				{
					format(string, sizeof(string), "%s (%d) não está em um carro!", GetPlayerNameEx(plid), plid);
					SendClientMessage(playerid, Vermelho, string);
				}
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Apenas ADM's podem usar este comando!");
		}
		return 1;
	}

	if(strcmp(cmd, "/injetar", true) == 0)
	{
		if(pAdmin[playerid] > 0)
		{
			new plid, VehicleID;

			if(sscanf(cmdtext, "s[9]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /injetar [id]");
				return 1;
			}
			if(plid == playerid)
			{
				SendClientMessage(playerid, Vermelho, "Você não pode dar uma carona para você mesmo.");
				return 1 ;
			}
			if(IsPlayerConnected(plid))
			{
				if(IsPlayerInAnyVehicle(playerid))
				{
					VehicleID = GetPlayerVehicleID(playerid);
					PutPlayerInVehicle(plid, VehicleID, 1);

					format(string, sizeof(string), "O(A) ADM %s (%d) te deu uma carona!", GetPlayerNameEx(playerid), playerid);
					SendClientMessage(plid, tcadm, string);

					SendClientMessage(playerid, Verde, "Comando efetuado com sucesso!");
				}
				else
				{
					SendClientMessage(playerid, Vermelho, "Você não está em um carro!");
				}
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "O(A) jogador(a) não está conectado.");
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Apenas ADM's podem usar este comando!");
		}
		return 1;
	}

	if(strcmp(cmd, "/ejetar", true) == 0)
	{
		if(pAdmin[playerid] > 1)
		{
			new plid;

			if(sscanf(cmdtext, "s[8]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /ejetar [id]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				if(IsPlayerInAnyVehicle(plid))
				{
					RemovePlayerFromVehicle(plid);

					format(string, sizeof(string), "O(A) ADM %s (%d) te tirou do veículo!", GetPlayerNameEx(playerid), playerid);
					SendClientMessage(plid, tcadm, string);

					SendClientMessage(playerid, Verde, "Comando efetuado com sucesso!");
				}
				else
				{
					format(string, sizeof(string), "%s (%d) não está em um carro!", GetPlayerNameEx(plid), plid);
					SendClientMessage(playerid, Vermelho, string);
				}
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente!");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/desbugar", true) == 0)
	{
		if(pAdmin[playerid] > 1)
		{
			new plid;

			if(sscanf(cmdtext, "s[10]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /desbugar [id]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				#if defined IntroTextUser
				ReconnectPlayer(plid);
				format(string, sizeof(string), "O(A) ADM %s (%d) desbugou o player: %s (%d)!", GetPlayerNameEx(playerid), playerid, GetPlayerNameEx(plid), plid);
				SendClientMessageToAll(tcadm, string);
				#else
				SendClientMessage(playerid, Vermelho, "Comando desativado no momento, o player foi notificado a relogar-se!");
				SendClientMessage(plid, Vermelho, "(ATENÇÃO) Se você estiver bugado relogue imediatamente.");
				#endif
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente!");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/abencoar", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Religioso || pAdmin[playerid] > 1)
		{
			new plid;

			if(sscanf(cmdtext, "s[10]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /abencoar [id]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				format(file2, sizeof(file2), PASTA_CONTAS, GetPlayerNameEx(plid));
				dini_IntSet(file2, "Dinheiro", 1000);

				format(string, sizeof(string), "O religioso %s abençoou você.", GetPlayerNameEx(playerid));
				SendClientMessage(playerid, C_Religioso, string);
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente!");
			}
		}
		return 1;
	}

	if(strcmp("/espiar", cmd, true) == 0)
	{
		if(pAdmin[playerid] > 0)
		{
			new specplayerid;

			if(sscanf(cmdtext, "s[8]u", cmd, specplayerid))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /espiar [id]");
				return 1;
			}
			if(!IsPlayerConnected(specplayerid))
			{
				SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente!");
				return 1;
			}

			gSpectateID[playerid] = specplayerid;
			TogglePlayerSpectating(playerid, 1);
			SetPlayerInterior(playerid, GetPlayerInterior(specplayerid));

			if(IsPlayerInAnyVehicle(specplayerid))
			{
				PlayerSpectateVehicle(playerid, GetPlayerVehicleID(specplayerid));
				gSpectateType[playerid] = ADMIN_SPEC_TYPE_VEHICLE;
			}
			else
			{
				PlayerSpectatePlayer(playerid, specplayerid);
				gSpectateType[playerid] = ADMIN_SPEC_TYPE_PLAYER;
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/pararespiar", true) == 0)
	{
		if(pAdmin[playerid] > 0)
		{
			if(GetPlayerState(playerid) == PLAYER_STATE_SPECTATING)
			{
				TogglePlayerSpectating(playerid, 0);
				gSpectateID[playerid] = INVALID_PLAYER_ID;
				gSpectateType[playerid] = ADMIN_SPEC_TYPE_NONE;
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/explodircarp", true) == 0)
	{
		if(pAdmin[playerid] == 5)
		{
			new plid;

			if(sscanf(cmdtext, "s[14]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /explodircarp [id]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				KillVehicle(GetPlayerVehicleID(plid));

				format(string, sizeof(string), "O(A) ADM %s (%d) explodiu o seu veículo!", GetPlayerNameEx(playerid), playerid);
				SendClientMessage(plid, tcadm, string);

				SendClientMessage(playerid, Verde, "Comando efetuado com sucesso!");
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente!");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/abrirconta", true) == 0)
	{
		if(IsPlayerInDynamicCP(playerid, CheckpointsFix[36]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[37]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[38]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[39]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[40]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[41]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[42])
			|| IsPlayerInDynamicCP(playerid, CheckpointsFix[43]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[52]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[53]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[54]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[55]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[56]))
		{
			format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
			if(dini_Int(file, "ContaBancaria") == 0)
			{
				if(GetPlayerGrana(playerid) > 299)
				{
					GivePlayerGrana(playerid, -300);

					dini_IntSet(file, "ContaBancaria", 1);
					dini_IntSet(file, "SaldoBancario", 300);

					format(string, sizeof(string), "O(A) jogador(a) %s (ID: %d) abriu uma conta bancária.", GetPlayerNameEx(playerid), playerid);
					SendClientMessageToAll(roxo, string);
				}
				else
				{
					SendClientMessage(playerid, Vermelho, "Para abrir uma conta bancária você presisa de pelo menos $300.");
				}
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Você já tem uma conta bancária.");
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não está em um banco.");
		}
		return 1;
	}

	if(strcmp(cmd, "/saldo", true) == 0)
	{
		if(IsPlayerInDynamicCP(playerid, CheckpointsFix[36]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[37]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[38]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[39]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[40]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[41]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[42])
			|| IsPlayerInDynamicCP(playerid, CheckpointsFix[43]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[52]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[53]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[54]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[55]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[56]))
		{
			format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
			if(dini_Int(file, "ContaBancaria") == 1)
			{
				format(string, sizeof(string), "Banco: Você tem depositado em sua conta $%d.", dini_Int(file, "SaldoBancario"));
				SendClientMessage(playerid, Verde, string);
			}
			else
			{
				SendClientMessage(playerid, Vermelho,"Você não tem uma conta bancária.");
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não está em um banco.");
		}
		return 1;
	}

	if(strcmp(cmd, "/saldocell", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "ContaBancaria") == 1)
		{
			format(string, sizeof(string), "Secretaria eletrônica: Você tem depositado no banco $%d.", dini_Int(file, "SaldoBancario"));
			SendClientMessage(playerid, Verde, string);
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem uma conta bancária.");
		}
		return 1;
	}

	if(strcmp(cmd, "/depositar", true) == 0)
	{
		if(IsPlayerInDynamicCP(playerid, CheckpointsFix[36]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[37]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[38]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[39]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[40]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[41]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[42]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[43])
			|| IsPlayerInDynamicCP(playerid, CheckpointsFix[52]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[53]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[54]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[55]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[56]))
		{
			new valor;

			if(sscanf(cmdtext, "s[11]d", cmd, valor))
			{
				SendClientMessage(playerid, Vermelho, "Use /depositar [quantia]");
				return 1;
			}
			format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
			if(dini_Int(file, "ContaBancaria") == 1)
			{
				if(GetPlayerGrana(playerid) > valor-1 && valor > 0)
				{
					GivePlayerGrana(playerid, -valor);
					dini_IntSet(file, "SaldoBancario", dini_Int(file, "SaldoBancario")+valor);

					format(string, sizeof(string), "Banco: Você depositou a quantia de $%d.", valor);
					SendClientMessage(playerid, Verde, string);
				}
				else
				{
					SendClientMessage(playerid, Vermelho, "Você não tem todo este dinheiro.");
				}
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Você não tem uma conta bancária.");
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não está em um banco.");
		}
		return 1;
	}

	if(strcmp(cmd, "/sacar", true) == 0)
	{
		if(IsPlayerInDynamicCP(playerid, CheckpointsFix[36]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[37]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[38]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[39]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[40]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[41]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[42])
			|| IsPlayerInDynamicCP(playerid, CheckpointsFix[43]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[52]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[53]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[54]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[55]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[56]))
		{
			new valor;

			if(sscanf(cmdtext, "s[7]d", cmd, valor))
			{
				SendClientMessage(playerid, Vermelho, "Use /sacar [quantia]");
				return 1;
			}
			format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
			if(dini_Int(file, "ContaBancaria") == 1)
			{
				if(dini_Int(file, "SaldoBancario") > valor-1 && valor > 0)
				{
					dini_IntSet(file, "SaldoBancario", dini_Int(file, "SaldoBancario")-valor);
					GivePlayerGrana(playerid, valor);

					format(string, sizeof(string), "Banco: Você sacou a quantia de $%d.", valor);
					SendClientMessage(playerid, Verde, string);
				}
				else
				{
					SendClientMessage(playerid, Vermelho, "Você não tem todo este dinheiro.");
				}
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Você não tem uma conta bancária.");
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não está em um banco.");
		}
		return 1;
	}

	if(strcmp(cmd, "/repararp", true) == 0)
	{
		if(pAdmin[playerid] > 0)
		{
			new plid;

			if(sscanf(cmdtext, "s[10]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /repararp [id]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				ResprayVehicle(GetPlayerVehicleID(plid));

				format(string, sizeof(string), "O(A) ADM %s (%d) conscertou o seu veículo!", GetPlayerNameEx(playerid), playerid);
				SendClientMessage(plid, tcadm, string);

				SendClientMessage(playerid, Verde, "Comando efetuado com sucesso!");
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente!");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/deletcarp", true) == 0)
	{
		if(pAdmin[playerid] == 5)
		{
			new plid;

			if(sscanf(cmdtext, "s[11]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /deletcarp [id]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				Loop(o, sizeof(VehiclesFix))
				{
					if(GetPlayerVehicleID(plid) == VehiclesFix[o])
					{
						SendClientMessage(playerid, Vermelho, "O veículo em que o player está não pode ser deletado!");
						return 1;
					}
				}
				for(new carro = 0; carro < MAX_CONCES; carro++)
				{
					format(string, sizeof(string), PASTA_CONCE, carro);
					if(GetPlayerVehicleID(plid) == dini_Int(string, "Id"))
					{
						SendClientMessage(playerid, Vermelho, "Este carro não pode ser deletado!");
						return 1;
					}
				}
				DestroyVehicle(GetPlayerVehicleID(plid));

				format(string, sizeof(string), "O(A) ADM %s (%d) deletou o seu veículo!", GetPlayerNameEx(playerid), playerid);
				SendClientMessage(plid, tcadm, string);

				SendClientMessage(playerid, Verde, "Comando efetuado com sucesso!");
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente!");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/respawncarp", true) == 0)
	{
		if(pAdmin[playerid] > 0)
		{
			new plid;

			if(sscanf(cmdtext, "s[13]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /respawncarp [id]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				SetVehicleToRespawn(GetPlayerVehicleID(plid));

				format(string, sizeof(string), "O(A) ADM %s (%d) resetou seu carro!", GetPlayerNameEx(playerid), playerid);
				SendClientMessage(plid, tcadm, string);

				SendClientMessage(playerid, Verde, "Comando efetuado com sucesso!");
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente!");
			}
		}
		else
		{
			SendClientMessage(playerid, Verde, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp("/cv", cmd, true) == 0)
	{
		if(pAdmin[playerid] > 0 || IsPlayerVIP(playerid))
		{
			new modelo, carro, cor1, cor2,
				Float:X, Float:Y, Float:Z, Float:Angle;

			if(sscanf(cmdtext, "s[4]dD(-1)D(-1)", cmd, modelo, cor1, cor2))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /cv [modelo] [cor1] [cor2]");
				return 1;
			}
			if(IsPlayerConnected(playerid))
			{
				if(modelo >= 400 && modelo <= 611)
				{
					if(modelo == 425 || modelo == 469 || modelo == 538 || modelo == 537 || modelo == 520 || modelo == 449 || modelo == 447 || modelo == 569 || modelo == 570 || modelo == 432)
					{
						SendClientMessage(playerid, tcadm, "Veículo proibido, tente outro! | ID's = 400-611");
						return 1;
					}
					if(IsPlayerInAnyVehicle(playerid))
					{
						SendClientMessage(playerid, Vermelho, "Saia deste veículo para criar outro.");
						return 1;
					}
					GetPlayerPos(playerid, X, Y, Z);
					GetPlayerFacingAngle(playerid, Angle);

					carro = AddStaticVehicleEx(modelo, X, Y, Z, Angle, cor1, cor2, 30);
					PutPlayerInVehicle(playerid, carro, 0);

					LinkVehicleToInterior(carro, GetPlayerInterior(playerid));
					SetVehicleVirtualWorld(carro, GetPlayerVirtualWorld(playerid));

					VehicleOldID[playerid] = carro;
					crioucarro[playerid] = 1;

					format(string, sizeof(string), "Você criou o veículo de id: %d", modelo);
					SendClientMessage(playerid, tcadm, string);
				}
				else
				{
					SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente! | ID's = 400-611");
				}
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/skin", true) == 0)
	{
		if(pAdmin[playerid] > 0)
		{
			new plid, skin;

			if(sscanf(cmdtext, "s[6]ud", cmd, plid, skin))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /skin [id] [skin]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				if(skin >= 0 && skin < 299)
				{
					if(skin == 292 || skin == 271 || skin == 272 || skin == 273 || skin == 270 || skin == 269 || skin == 274 || skin == 289)
					{
						SendClientMessage(playerid, Vermelho, "Skin proibido, tente outro! | ID's = 0-298");
						return 1;
					}
					SetPlayerSkin(plid, skin);

					format(string, sizeof(string), "O ADM %s (%d) alterou sua skin para: %d", GetPlayerNameEx(playerid), playerid, skin);
					SendClientMessage(plid,tcadm, string);

					SendClientMessage(playerid, Verde, "Comando efetuado com sucesso!");
				}
				else
				{
					SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente! | ID's = 0-298");
				}
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Jogador não conectado!");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/vidaveiculo", true) == 0)
	{
		if(pAdmin[playerid] > 2)
		{
			new plid, Float:vidacarro;

			if(sscanf(cmdtext, "s[13]uf", cmd, plid, vidacarro))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /vidaveiculo [id] [vida]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				if(IsPlayerInAnyVehicle(plid))
				{
					FreezeVehicleHealth(GetPlayerVehicleID(plid), vidacarro, 1);

					format(string, sizeof(string), "%s (%d) setou a vida do seu veículo para: %f", GetPlayerNameEx(playerid), playerid, vidacarro);
					SendClientMessage(plid, tcadm, string);

					SendClientMessage(playerid, Verde, "Comando efetuado com sucesso!");
				}
				else
				{
					SendClientMessage(playerid, Vermelho, "O(A) jogador(a) não está em um veículo.");
				}
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Jogador não conectado!");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/dararma", true) == 0)
	{
		if(pAdmin[playerid] > 2)
		{
			new plid, idarma, municao;

			if(sscanf(cmdtext, "s[9]udd", cmd, plid, idarma, municao))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /dararma [id] [arma] [munição]");
				return 1;
			}
			if(idarma == 38 || idarma == 35 || idarma == 36 || idarma == 37 || idarma == 39 || idarma == 40)
			{
				SendClientMessage(playerid, Vermelho, "ID de arma proibida!");
				return 1;
			}
			if(municao > 9000)
			{
				SendClientMessage(playerid, Vermelho, "A munição máxima permitida é 9000.");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				GivePlayerWeapon(plid, idarma, municao);

				format(string, sizeof(string), "%s (%d) te forneceu uma arma de ID: %d, Munição: %d", GetPlayerNameEx(playerid), playerid, idarma, municao);
				SendClientMessage(plid, tcadm, string);

				SendClientMessage(playerid, Verde, "Comando efetuado com sucesso!");
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Jogador não conectado!");
			}
		}
		return 1;
	}

	if(strcmp("/ann", cmd, true) == 0)
	{
		if(pAdmin[playerid] > 0)
		{
			new texto[64];

			if(sscanf(cmdtext, "s[5]s[64]", cmd, texto))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /ann [texto]");
				return 1;
			}
			format(string, sizeof(string), "~g~] ~w~%s ~g~]", texto);
			GameTextForAll(string, 5000, 3);
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão para usar este comando!");
		}
		return 1;
	}

	if(strcmp("/cnn", cmd, true) == 0)
	{
		if(pAdmin[playerid] > 0)
		{
			new texto[64];

			if(sscanf(cmdtext, "s[5]s[64]", cmd, texto))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /cnn [texto]");
				return 1;
			}
			format(string, sizeof(string), "%s diz:~n~~g~] ~w~%s ~g~]", GetPlayerNameEx(playerid), texto);
			GameTextForAll(string, 5000, 3);
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão para usar este comando!");
		}
		return 1;
	}

	if(strcmp(cmd, "/resetargrana", true) == 0)
	{
		if(pAdmin[playerid] > 2)
		{
			new plid;

			if(sscanf(cmdtext, "s[14]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /resetargrana [id]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				ResetPlayerGrana(plid);

				format(string, sizeof(string), "O(A) ADM %s (%d) resetou sua grana!", GetPlayerNameEx(playerid), playerid);
				SendClientMessage(plid, tcadm, string);

				SendClientMessage(playerid, Verde, "Comando efetuado com sucesso!");
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Jogador não conectado!");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/resetarsaldo", true) == 0)
	{
		if(pAdmin[playerid] > 2)
		{
			new plid;

			if(sscanf(cmdtext, "s[14]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /resetarsaldo [id]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(plid));
				dini_IntSet(file, "SaldoBancario", 300);

				format(string, sizeof(string), "O(A) ADM %s (%d) resetou seu saldo no banco!", GetPlayerNameEx(playerid), playerid);
				SendClientMessage(plid, tcadm, string);

				SendClientMessage(playerid, Verde, "Comando efetuado com sucesso!");
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Jogador não conectado!");
			}
		}
		return 1;
	}

	if(strcmp("/setadmin", cmd, true) == 0)
	{
		#if defined SystemAdminUser
		if(PlayerInfo[playerid][SCON] == true)
		{
			new plid, lvl;

			if(sscanf(cmdtext, "s[10]ud", cmd, plid, lvl))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /setadmin [id] [level]");
				return 1;
			}
			if(lvl >= 6)
			{
				SendClientMessage(playerid, Vermelho, "O level permitido para ADM é de 0 a 5!");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				PlayerSetPlayerAdmin(playerid, plid, lvl);
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Jogador não conectado!");
			}
		}
		#else
		SendClientMessage(playerid, Azul, "Comando desativado temporáriamente.");
		#endif

		return 1;
	}

	if(strcmp(cmd, "/tiraradmdetodos", true) == 0)
	{
		if(!PlayerInfo[playerid][SCON] == true)
		{
			return 1;
		}
		for(new i = 0; i < MAX_PLAYERS; i++)
		{
			if(IsPlayerConnected(i))
			{
				if(pAdmin[i] > 0)
				{
					PlayerSetPlayerAdmin(-1, i, 0);
				}
			}
		}
		SendClientMessage(playerid, Verde, "Concluído somente para os Online!");
		return 1;
	}

	if(strcmp(cmd, "/admins", true) == 0)
	{
		new str[1000], count = 0;

		for(new i = 0; i < MAX_PLAYERS; i++)
		{
			if(IsPlayerConnected(i))
			{
				if(admin[i])
				{
					if(pAdmin[i] == 1)
					{
						format(str, sizeof(str), "{FFFFFF}%s (%d) {FF0000}:: {FFFFFF}[MOD]\n", GetPlayerNameEx(i), i);
						strcat(string, str, sizeof(string));
					}
					if(pAdmin[i] == 2)
					{
						format(str, sizeof(str), "{FFFFFF}%s (%d) {FF0000}:: {FFFFFF}[ADM]\n", GetPlayerNameEx(i), i);
						strcat(string, str, sizeof(string));
					}
					if(pAdmin[i] == 3)
					{
						format(str, sizeof(str), "{FFFFFF}%s (%d) {FF0000}:: {FFFFFF}[Chefe]\n", GetPlayerNameEx(i), i);
						strcat(string, str, sizeof(string));
					}
					if(pAdmin[i] == 4)
					{
						format(str, sizeof(str), "{FFFFFF}%s (%d) {FF0000}:: {FFFFFF}[Guardião]\n", GetPlayerNameEx(i), i);
						strcat(string, str, sizeof(string));
					}
					if(pAdmin[i] == 5)
					{
						format(str, sizeof(str), "{FFFFFF}%s (%d) {FF0000}:: {FFFFFF}[Dono]\n", GetPlayerNameEx(i), i);
						strcat(string, str, sizeof(string));
					}
					count++;
				}
			}
		}
		if(count == 0)
		{
			ShowPlayerDialog(playerid, playersimp, DIALOG_STYLE_MSGBOX, ".:: - ADM's Online - ::.", "{FF0000}Não há ADM's online no momento.", "OK", "");
		}
		else
		{
			ShowPlayerDialog(playerid, playersimp, DIALOG_STYLE_MSGBOX, ".:: - ADM's Online - ::.", string, "OK", "");
		}
		return 1;
	}

	if(strcmp(cmd, "/comandosvip", true) == 0)
	{
		if(IsPlayerConnected(playerid))
		{
			if(IsPlayerVIP(playerid))
			{
				strcat(string, "{00FF00}>: /eusouvip /virar /comemorar /hydra /irpos /cvp\n", sizeof(string));
				strcat(string, "{00FF00}>: /tunar  /armas /bazuca\n", sizeof(string));
				ShowPlayerDialog(playerid, playersimp, DIALOG_STYLE_MSGBOX, ".:: - Comandos VIP - ::.", string, "OK", "");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/menuadm", true) == 0)
	{
		if(pAdmin[playerid] > 1)
		{
			ShowPlayerDialog(playerid, menuadm, DIALOG_STYLE_LIST, ".:: - Menu ADM - ::.", STRD_MENUADM, "OK", "SAIR");
		}
		return 1;
	}

	if(strcmp(cmd, "/aa", true) == 0)
	{
		new strcmd[1000];

		if(IsPlayerConnected(playerid))
		{
			if(pAdmin[playerid] == 1)
			{
				strcat(strcmd, "{00FF00}>: /respawncar /vida /colete /cercar /descercar\n", sizeof(strcmd));
				strcat(strcmd, "{00FF00}>: /trazer /desbugar /espiar /banip /lc\n", sizeof(strcmd));
				strcat(strcmd, "{00FF00}>: /virar /repararp /skin /darlevel /ann /carona\n", sizeof(strcmd));
				strcat(strcmd, "{00FF00}>: /coletet /evento /pararespiar /darestudo /a\n", sizeof(strcmd));
				strcat(strcmd, "{00FF00}>: /cnn /vidat /contar /ingetar /comemorar /ir\n", sizeof(strcmd));
				ShowPlayerDialog(playerid, playersimp, DIALOG_STYLE_MSGBOX, ".:: - Comandos ADM 1 - ::.", strcmd, "OK", "");
			}
			if(pAdmin[playerid] == 2)
			{
				strcat(strcmd, "{00FF00}>: /respawncar /recuperar /descalar /resetarflood /cercar\n", sizeof(strcmd));
				strcat(strcmd, "{00FF00}>: /lc /colete /calar /trazer /contar /ingetar /descercar\n", sizeof(strcmd));
				strcat(strcmd, "{00FF00}>: /descongelar /desarmar /desbugar /comemorar /banip /jetpack\n", sizeof(strcmd));
				strcat(strcmd, "{00FF00}>: /virar /pararespiar /repararp /ejetar /coletet /darestudo\n", sizeof(strcmd));
				strcat(strcmd, "{00FF00}>: /espiar /ircarro /respawncarp /punir /congelar /evento /ir\n", sizeof(strcmd));
				strcat(strcmd, "{00FF00}>: /carona /skin /kick /ban /vidat /darlevel /vida /ann /cnn /a\n", sizeof(strcmd));
				strcat(strcmd, "\n{FF0000}>:\t/menuadm", sizeof(strcmd));
				ShowPlayerDialog(playerid, playersimp, DIALOG_STYLE_MSGBOX, ".:: - Comandos ADM 2 - ::.", strcmd, "OK", "");
			}
			if(pAdmin[playerid] == 3)
			{
				strcat(strcmd, "{00FF00}>: /virar /respawncar /cnn /punir /darlevel /blockpm /banip /ejetar\n", sizeof(strcmd));
				strcat(strcmd, "{00FF00}>: /recuperar /jetpack /resetargrana /resetarsaldo /colete /trazer\n", sizeof(strcmd));
				strcat(strcmd, "{00FF00}>: /skin /desbugar /congelar /descongelar /desarmar /lc /comemorar\n", sizeof(strcmd));
				strcat(strcmd, "{00FF00}>: /ban /espiar /pararespiar /repararp /ir /vidat /resetarflood /a\n", sizeof(strcmd));
				strcat(strcmd, "{00FF00}>: /respawncarp /clima /ircarro /coletet /desarmart /darestudo /carona\n", sizeof(strcmd));
				strcat(strcmd, "{00FF00}>: /dararma /ann /liberarnick /amod /trazertodos /cercar /descercar /ip\n", sizeof(strcmd));
				strcat(strcmd, "{00FF00}>: /vidaveiculo /kick /evento /ingetar /contar /calar /descalar /vida\n", sizeof(strcmd));
				strcat(strcmd, "\n{FF0000}>:\t/menuadm", sizeof(strcmd));
				ShowPlayerDialog(playerid, playersimp, DIALOG_STYLE_MSGBOX, ".:: - Comandos ADM 3 - ::.", strcmd, "OK", "");
			}
			if(pAdmin[playerid] == 4)
			{
				strcat(strcmd, "{00FF00}>: /ip /virar /respawncar /contar /ircarro /resetarflood /liberarchat\n", sizeof(strcmd));
				strcat(strcmd, "{00FF00}>: /recuperar /jetpack /colete /trazer /fakeban /infoban /banip /lc /cnn\n", sizeof(strcmd));
				strcat(strcmd, "{00FF00}>: /congelar /descongelar /desarmar /ejetar /darestudo /blockchat /ann\n", sizeof(strcmd));
				strcat(strcmd, "{00FF00}>: /desbugar /espiar /pararespiar /repararp /ingetar /blockpm /rvc /rvu /a\n", sizeof(strcmd));
				strcat(strcmd, "{00FF00}>: /deletcar /respawncarp /skin /dargrana /comemorar /punir /vida /clima\n", sizeof(strcmd));
				strcat(strcmd, "{00FF00}>: /vidaveiculo /dararma /calar /descalar /resetargrana /resetarsaldo\n", sizeof(strcmd));
				strcat(strcmd, "{00FF00}>: /ban /desbanir /trazertodos /liberarnick /darlevel /descercar /ir /rv\n", sizeof(strcmd));
				strcat(strcmd, "{00FF00}>: /kick /vidat /coletet /desarmart /amod /evento /carona /cercar /cv\n", sizeof(strcmd));
				strcat(strcmd, "\n{FF0000}>:\t/menuadm", sizeof(strcmd));
				ShowPlayerDialog(playerid, playersimp, DIALOG_STYLE_MSGBOX, ".:: - Comandos ADM 4 - ::.", strcmd, "OK", "");
			}
			if(pAdmin[playerid] == 5)
			{
				strcat(strcmd, "{00FF00}>: /virar /evento /deletcar /respawncar /explodir /clima /fakeban /banip /descercar /skin\n", sizeof(strcmd));
				strcat(strcmd, "{00FF00}>: /calar /recuperar /godmod /godcar /jetpack /vida /colete /desbugar /blockpm /infoban /cercar\n", sizeof(strcmd));
				strcat(strcmd, "{00FF00}>: /rmov /desarmar /espiar /trazer /ir /descongelar /contar /darlevel /vercmds /carona /rv /lc\n", sizeof(strcmd));
				strcat(strcmd, "{00FF00}>: /congelar /pararespiar /explodircarp /objstatus /vehstatus /liberarchat /blockchat /cnn /a\n", sizeof(strcmd));
				strcat(strcmd, "{00FF00}>: /deletcarp /tapa /respawncarp /salvarcar /comemorar /ircarro /reconectarnpcs /rvc /rvu /cv /ip\n", sizeof(strcmd));
				strcat(strcmd, "{00FF00}>: /descalar /vidaveiculo /dararma /dargrana /liberardm /blockdm /resetargrana /resetarsaldo\n", sizeof(strcmd));
				strcat(strcmd, "{00FF00}>: /ban /banirnick /desbanirnick /desbanir /liberarnick /punir /ingetar /resetarflood /ann\n", sizeof(strcmd));
				strcat(strcmd, "{00FF00}>: /kick /vidat /coletet /desarmart /trazertodos /darestudo /amod /ejetar /repararp /irpos\n", sizeof(strcmd));
				strcat(strcmd, "\n{FF0000}>:\t/menuadm", sizeof(strcmd));
				ShowPlayerDialog(playerid, playersimp, DIALOG_STYLE_MSGBOX, ".:: - Comandos ADM 5 - ::.", strcmd, "OK", "");
			}
		}
		return 1;
	}

	if(strcmp("/a", cmd, true) == 0)
	{
		if(pAdmin[playerid] > 0)
		{
			new texto[128];

			if(sscanf(cmdtext, "s[4]s[128]", cmd, texto))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /a [texto]");
			}
			else
			{
				format(string, sizeof(string), "(») Admin («) %s diz: %s", GetPlayerNameEx(playerid), texto);
				SendClientMessageToAll(roxo, string);
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp("/ca", cmd, true) == 0)
	{
		if(pAdmin[playerid] > 0)
		{
			new texto[128];

			if(sscanf(cmdtext, "s[4]s[128]", cmd, texto))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /ca [texto]");
			}
			else
			{
				format(string, sizeof(string), "(») Chat-Admin («) %s (%d) diz: %s", GetPlayerNameEx(playerid), playerid, texto);
				ABroadCast(Amarelo, string, 1);
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão para usar este comando!");
		}
		return 1;
	}

	if(strcmp("/cd", cmd, true) == 0)
	{
		if(pAdmin[playerid] == 5)
		{
			new texto[128];

			if(sscanf(cmdtext, "s[4]s[128]", cmd, texto))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /cd [texto]");
			}
			else
			{
				format(string, sizeof(string), "(») Chat-Dono («) %s (%d) diz: %s", GetPlayerNameEx(playerid), playerid, texto);
				ABroadCast(Amarelo, string, 5);
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão para usar este comando!");
		}
		return 1;
	}

	if(strcmp("/cvp", cmd, true) == 0)
	{
		if(IsPlayerVIP(playerid))
		{
			new texto[128];

			if(sscanf(cmdtext, "s[4]s[128]", cmd, texto))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /cvp [texto]");
			}
			else
			{
				format(string, sizeof(string), "(») Chat-VIP («) %s (%d) diz: %s", GetPlayerNameEx(playerid), playerid, texto);
				ABroadCastVIP(-1, string);
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão para usar este comando!");
		}
		return 1;
	}

	if(strcmp(cmd, "/armas", true) == 0)
	{
		if(pAdmin[playerid] > 3 || IsPlayerVIP(playerid))
		{
			ShowPlayerDialog(playerid, menuarmas, DIALOG_STYLE_LIST, "Armas Vip", "Pistolas\nMicro Smg\nShotguns\nSMG\nRifles\nAssalto\nOutras", "OK", "Cancelar");
		}
		return 1;
	}

	if(strcmp(cmd, "/minigun", true) == 0)
	{
		if(pAdmin[playerid] > 3)
		{
			GivePlayerWeapon(playerid, 38, 9000);
		}
		return 1;
	}

	if(strcmp(cmd, "/bazuca", true) == 0)
	{
		if(pAdmin[playerid] > 3 || IsPlayerVIP(playerid))
		{
			GivePlayerWeapon(playerid, 35, 9000);
		}
		return 1;
	}

	if(strcmp(cmd, "/hydra", true) == 0)
	{
		if(pAdmin[playerid] > 3)
		{
			new carro, Float:X, Float:Y, Float:Z, Float:Angle;

			if(IsPlayerInAnyVehicle(playerid))
			{
				SendClientMessage(playerid, Vermelho, "Saia deste veículo para criar outro.");
				return 1;
			}
			GetPlayerPos(playerid, X, Y, Z);
			GetPlayerFacingAngle(playerid, Angle);

			carro = AddStaticVehicleEx(520, X, Y, Z, Angle, -1, -1, 30);
			PutPlayerInVehicle(playerid, carro, 0);

			LinkVehicleToInterior(carro, GetPlayerInterior(playerid));
			SetVehicleVirtualWorld(carro, GetPlayerVirtualWorld(playerid));

			VehicleOldID[playerid] = carro;
			crioucarro[playerid] = 1;
		}
		return 1;
	}

	if(strcmp(cmd, "/relatorio", true) == 0)
	{
		if(IsPlayerConnected(playerid))
		{
			new texto[128];

			if(sscanf(cmdtext, "s[11]s[128]", cmd, texto))
			{
				SendClientMessage(playerid, Vermelho, "Use /relatorio [texto]");
				return 1;
			}
			if(IsPlayerVIP(playerid))
			{
				format(string, sizeof(string), "Relatorio ((VIP)) de %s (ID: %d): %s", GetPlayerNameEx(playerid), playerid, texto);
				ABroadCast(Violeta, string, 1);

				SendClientMessage(playerid, Aviso, "Seu relatorio 'VIP' foi enviado a administração com sucesso, aguarde uma resposta!");
			}
			else
			{
				format(string, sizeof(string), "Relatorio de %s (ID: %d): %s", GetPlayerNameEx(playerid), playerid, texto);
				Relatorio(0xFF8000AA, string, 1);

				SendClientMessage(playerid, Aviso, "Seu relatorio foi enviado a administração com sucesso, aguarde uma resposta!");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/exame", true) == 0)
	{
		new plid;

		if(sscanf(cmdtext, "s[7]u", cmd, plid))
		{
			SendClientMessage(playerid, COLOR_GREEN, "Use /exame [id]");
			return 1;
		}
		if(IsPlayerConnected(plid))
		{
			if(GetDistanceBetweenPlayers(plid, playerid) < 21)
			{
				format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(plid));

				SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~ Exame ~~~~~~~~~~~~~~");

				format(string, sizeof(string), "- Maconha: %d", dini_Int(file, "Maconha"));
				SendClientMessage(playerid, 0xFFFFFFAA, string);

				format(string, sizeof(string), "- Crack: %d", dini_Int(file, "Crack"));
				SendClientMessage(playerid, 0xFFFFFFAA, string);

				format(string, sizeof(string), "- Cocaina: %d", dini_Int(file, "Cocaina"));
				SendClientMessage(playerid, 0xFFFFFFAA, string);

				SendClientMessage(playerid, Vermelho, "~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~");

				format(string, sizeof(string), "- Maconha no sangue: %d", dini_Int(file, "tmaconha"));
				SendClientMessage(playerid, 0xFFFFFFAA, string);

				format(string, sizeof(string), "- Crack no sangue: %d", dini_Int(file, "tcrack"));
				SendClientMessage(playerid, 0xFFFFFFAA, string);

				format(string, sizeof(string), "- Cocaína no sangue: %d", dini_Int(file, "tcocaina"));
				SendClientMessage(playerid, 0xFFFFFFAA, string);

				SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~ Exame ~~~~~~~~~~~~~~");

				format(string, sizeof(string), "Você está examinando o(a) jogador(a) %s ", GetPlayerNameEx(plid));
				SendClientMessage(playerid, COLOR_GREEN, string);
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Chega mais perto para examinar!");
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente!");
		}
		return 1;
	}

	if(strcmp("/minhasdrogas", cmd, true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Traficante)
		{
			SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~");

			format(string, sizeof(string), "Maconha: %d", dini_Int(file, "Maconha"));
			SendClientMessage(playerid, 0xFFFFFFAA, string);

			format(string, sizeof(string), "Crack: %d", dini_Int(file, "Crack"));
			SendClientMessage(playerid, 0xFFFFFFAA, string);

			format(string, sizeof(string), "Cocaina: %d", dini_Int(file, "Cocaina"));
			SendClientMessage(playerid, 0xFFFFFFAA, string);

			SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~");
		}
		return 1;
	}

	if(strcmp("/usarmaconha", cmd, true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "tmaconha") >= 1)
		{
			dini_IntSet(file, "tmaconha", dini_Int(file, "tmaconha")-1);
			dini_IntSet(file, "usoudroga", 1);

			TextDrawShowForPlayer(playerid, drogas1);
			TextDrawShowForPlayer(playerid, drogas2);

			SendClientMessage(playerid, C_Traficante, "Você usou maconha. Agora aguarde para ver os efeitos dela!");
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem Maconha.");
		}
		return 1;
	}

	if(strcmp("/usarcrack", cmd, true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "tcrack") >= 1)
		{
			dini_IntSet(file, "tcrack", dini_Int(file, "tcrack")-1);
			dini_IntSet(file, "usoudroga", 1);

			TextDrawShowForPlayer(playerid, drogas1);
			TextDrawShowForPlayer(playerid, drogas2);

			SendClientMessage(playerid, C_Traficante, "Você usou crack. Agora aguarde para ver os efeitos dele!");
		}
		else
		{
			SendClientMessage(playerid, Vermelho, " Você não tem Crack.");
		}
		return 1;
	}

	if(strcmp("/usarcocaina", cmd, true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "tcocaina") == 1)
		{
			dini_IntSet(file, "tcocaina", dini_Int(file, "tcocaina")-1);
			dini_IntSet(file, "usoudroga", 1);

			TextDrawShowForPlayer(playerid, drogas1);
			TextDrawShowForPlayer(playerid, drogas2);

			SendClientMessage(playerid, C_Traficante, "Você usou cocaína. Agora aguarde para ver os efeitos dela!");
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem Cocaína.");
		}
		return 1;
	}

	if(strcmp("/pegardrogas", cmd, true) == 0)
	{
		if(GetPlayerGrana(playerid) > 199)
		{
			if(IsPlayerInDynamicCP(playerid, CheckpointsFix[3]))
			{
				format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
				if(dini_Int(file, "Profissao") == Traficante || TraficanteD)
				{
					GivePlayerGrana(playerid, -200);

					dini_IntSet(file, "Maconha", dini_Int(file, "Maconha")+5);
					dini_IntSet(file, "Crack", dini_Int(file, "Crack")+5);
					dini_IntSet(file, "Cocaina", dini_Int(file, "Cocaina")+5);

					SendClientMessage(playerid, C_Traficante, "Você pegou as drogas, 5kg de Maconha, Cocaína e Crack.");
					SendClientMessage(playerid, C_Traficante, "Você pagou $200 por elas. /minhasdrogas");
				}
				else
				{
					SendClientMessage(playerid, C_Traficante, "Você não é um Traficante de Drogas!");
				}
			}
			else
			{
				SendClientMessage(playerid, C_Traficante, "Você não está na 'boca' de fumo e não pode pegar drogas!");
			}
		}
		else
		{
			SendClientMessage(playerid, C_Traficante, "Você não tem dinheiro suficiente. ($200)");
		}
		return 1;
	}

	if(strcmp(cmd, "/ofmaconha", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Traficante)
		{
			new plid;

			if(sscanf(cmdtext, "s[11]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /ofmaconha [id]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				format(file2, sizeof(file2), PASTA_CONTAS, GetPlayerNameEx(plid));
				if(dini_Int(file, "Maconha") == 0)
				{
					SendClientMessage(playerid, Vermelho, "Sua maconha acabou, pegue mais na boca!");
					return 1;
				}
				if(dini_Int(file, "Maconha") >= 1)
				{
					dini_IntSet(file, "Maconha", dini_Int(file, "Maconha")-1);
					dini_IntSet(file2, "ofmaconha", 1);

					format(string, sizeof(string), "%s te ofereceu 1kg de maconha!", GetPlayerNameEx(playerid));
					SendClientMessage(plid, C_Traficante, string);

					SendClientMessage(plid, C_Traficante, "Você pode aceitar a droga ou recusar: /aceitar /recusar.");
					SendClientMessage(playerid, Verde, "Maconha oferecida com sucesso!");
				}
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente!");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/ofcrack", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Traficante)
		{
			new plid;

			if(sscanf(cmdtext, "s[9]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /ofcrack [id]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				format(file2, sizeof(file2), PASTA_CONTAS, GetPlayerNameEx(plid));
				if(dini_Int(file, "Crack") == 0)
				{
					SendClientMessage(playerid, Vermelho, "Seu crack acabou, pegue mais na boca!");
					return 1;
				}
				if(dini_Int(file, "Crack") >= 1)
				{
					dini_IntSet(file, "Crack", dini_Int(file, "Crack")-1);
					dini_IntSet(file2, "ofcrack", 1);

					format(string, sizeof(string), "%s te ofereceu 1kg de crack!", GetPlayerNameEx(playerid));
					SendClientMessage(plid, C_Traficante, string);

					SendClientMessage(plid, C_Traficante, "Você pode aceitar a droga ou recusar: /aceitar /recusar.");
					SendClientMessage(playerid, Verde, "Crack oferecido com sucesso!");
				}
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente!");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/ofcocaina", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Traficante)
		{
			new plid;

			if(sscanf(cmdtext, "s[11]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /ofcocaina [id]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				format(file2, sizeof(file2), PASTA_CONTAS, GetPlayerNameEx(plid));
				if(dini_Int(file, "Cocaina") == 0)
				{
					SendClientMessage(playerid, Vermelho, "Sua cocaína acabou, pegue mais na boca!");
					return 1;
				}
				if(dini_Int(file, "Cocaina") >= 1)
				{
					dini_IntSet(file, "Cocaina", dini_Int(file, "Cocaina")-1);
					dini_IntSet(file2, "ofcocaina", 1);

					format(string, sizeof(string), "%s te ofereceu 1kg de cocaína!", GetPlayerNameEx(playerid));
					SendClientMessage(plid, C_Traficante, string);

					SendClientMessage(plid, C_Traficante, "Você pode aceitar a droga ou recusar: /aceitar /recusar.");
					SendClientMessage(playerid, Verde, "Cocaina oferecida com sucesso!");
				}
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente!");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/aceitar", true) == 0)
	{
		format(file2, sizeof(file2), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(IsPlayerConnected(playerid))
		{
			if(dini_Int(file2, "convitent") == 1)
			{
				SendClientMessage(playerid, Amarelo, "Você aceitou o convite para a entrevista!");
				SendClientMessage(playerid, Amarelo, "Vá até a prefeitura para começar a entrevista.");
			}
			if(dini_Int(file2, "ofmaconha") == 1)
			{
				dini_IntSet(file2, "ofmaconha", 0);
				dini_IntSet(file2, "tmaconha", dini_Int(file2, "tmaconha")+1);
				SendClientMessage(playerid, Amarelo, "Você tem mais 1kg de maconha, para usar digite /usarmaconha");
			}
			if(dini_Int(file2, "ofcrack") == 1)
			{
				dini_IntSet(file2, "ofcrack", 0);
				dini_IntSet(file2, "tcrack", dini_Int(file2, "tcrack")+1);
				SendClientMessage(playerid, Amarelo, "Você tem mais 1kg de crack, para usar digite /usarcrack!");
			}
			if(dini_Int(file2, "ofcocaina") == 1)
			{
				dini_IntSet(file2, "ofcocaina", 0);
				dini_IntSet(file2, "tcocaina", dini_Int(file2, "tcocaina")+1);
				SendClientMessage(playerid, Amarelo, "Você tem mais 1kg de cocaína, para usar digite /usarcocaina!");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/recusar", true) == 0)
	{
		format(file2, sizeof(file2), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(IsPlayerConnected(playerid))
		{
			if(dini_Int(file2, "convitent") == 1)
			{
				dini_IntSet(file2, "convitent", 0);
				SendClientMessage(playerid, Amarelo, "Você recusou o convite da entrevista!");

				format(string, sizeof(string), "%s (ID: %d) não quer ser entrevistado!", GetPlayerNameEx(playerid), playerid);
				SendClientMessageToAll(Vermelho, string);
			}
			if(dini_Int(file2, "ofmaconha") == 1)
			{
				dini_IntSet(file2, "ofmaconha", 0);
				SendClientMessage(playerid, Amarelo, "Você recusou mais 1kg de maconha!");
			}
			if(dini_Int(file2, "ofcrack") == 1)
			{
				dini_IntSet(file2, "ofcrack", 0);
				SendClientMessage(playerid, Amarelo, "Você recusou mais 1kg de crack!");
			}
			if(dini_Int(file2, "ofcocaina") == 1)
			{
				dini_IntSet(file2, "ofcocaina", 0);
				SendClientMessage(playerid, Amarelo, "Você recusou mais 1kg de cocaína!");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/preprr", true) == 0)
	{
		if(pAdmin[playerid] > 4)
		{
			prepbyid[playerid] = playerid;

			CreateCountdown(30, 1);
			PrepTimer = SetTimer("AutoRestart", 32000, 1);

			for(new i = 0; i < MAX_PLAYERS; i++)
			{
				if(IsPlayerConnected(i))
				{
					ClearChatbox(i, 5);
					SendClientMessage(i, Verde, "O servidor está sendo preparado para reniciar em 30 segundos.");
					SendClientMessage(i, Branco, "Após 'Server closed the connection.' aguarde, não precisa sair.");
					SendClientMessage(i, Azul, "Fique ligado conosco, use /salvar e ao relogar use /continuar.");
				}
			}

			SendClientMessage(playerid, Amarelo, "Para cancelar o processo use /cancelprep");
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem o direito de fazer isso.");
		}
		return 1;
	}

	if(strcmp(cmd, "/cancelprep", true) == 0)
	{
		if(pAdmin[playerid] > 4 && prepbyid[playerid] == playerid)
		{
			prepbyid[playerid] = INVALID_PLAYER_ID;

			StopCountdown();
			KillTimer(PrepTimer);

			for(new i = 0; i < MAX_PLAYERS; i++)
			{
				if(IsPlayerConnected(i))
				{
					format(string, sizeof(string), " %s (%d) cancelou o reiniciamento do servidor.", GetPlayerNameEx(i), playerid);
					SendClientMessage(i, tcadm, string);
				}
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem o direito de fazer isso.");
		}
		return 1;
	}

	if(strcmp(cmd, "/trazertodos", true) == 0)
	{
		if(pAdmin[playerid] > 2)
		{
			new Float:X, Float:Y, Float:Z;

			for(new i = 0; i < MAX_PLAYERS; i++)
			{
				if(IsPlayerConnected(i))
				{
					GetPlayerPos(playerid, X, Y, Z);

					SetPlayerInterior(i, GetPlayerInterior(playerid));
					SetPlayerPos(i, X+2, Y+2, Z);

					TogglePlayerControllable(i, false);
					SetTimerEx("DestogglePlayerControllable", 1000, 0, "d", i);

					format(string, sizeof(string), "O(A) ADM %s (%d) trouxe todos para sua posição.", GetPlayerNameEx(playerid), playerid);
					SendClientMessage(i, tcadm, string);
				}
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp(cmd, "/vidat", true) == 0)
	{
		if(pAdmin[playerid] > 0)
		{
			for(new i = 0; i < MAX_PLAYERS; i++)
			{
				if(IsPlayerConnected(i))
				{
					SetPlayerHealth(i, 100.0);
				}
			}
			format(string, sizeof(string), "O(A) ADM %s (%d) recuperou a vida de todos.", GetPlayerNameEx(playerid), playerid);
			SendClientMessageToAll(tcadm, string);
		}
		return 1;
	}

	if(strcmp(cmd, "/liberarchat", true) == 0)
	{
		if(pAdmin[playerid] > 3)
		{
			Chatlb = 1;
			format(string, sizeof(string), "O(A) ADM %s (%d) liberou o Chat Global!", GetPlayerNameEx(playerid), playerid);
			SendClientMessageToAll(tcadm, string);
		}
		return 1;
	}

	if(strcmp(cmd, "/blockchat", true) == 0)
	{
		if(pAdmin[playerid] > 3)
		{
			Chatlb = 0;
			format(string, sizeof(string), "O(A) ADM %s (%d) bloqueou o Chat Global!", GetPlayerNameEx(playerid), playerid);
			SendClientMessageToAll(tcadm, string);
		}
		return 1;
	}

	if(strcmp(cmd, "/liberardm", true) == 0)
	{
		if(pAdmin[playerid] == 5)
		{
			DMlb = 1;
			format(string, sizeof(string), "O(A) ADM %s (%d) liberou o DM!", GetPlayerNameEx(playerid), playerid);
			SendClientMessageToAll(tcadm, string);
		}
		return 1;
	}

	if(strcmp(cmd, "/blockdm", true) == 0)
	{
		if(pAdmin[playerid] > 4)
		{
			DMlb = 0;
			format(string, sizeof(string), "O(A) ADM %s (%d) bloqueou o DM!", GetPlayerNameEx(playerid), playerid);
			SendClientMessageToAll(tcadm, string);
		}
		return 1;
	}

	if(strcmp(cmd, "/coletet", true) == 0)
	{
		if(pAdmin[playerid] > 0)
		{
			for(new i = 0; i < MAX_PLAYERS; i++)
			{
				if(IsPlayerConnected(i))
				{
					SetPlayerArmour(i, 100.0);
				}
			}
			format(string, sizeof(string), "O(A) ADM %s (%d) recuperou o colete de todos.", GetPlayerNameEx(playerid), playerid);
			SendClientMessageToAll(tcadm, string);
		}
		return 1;
	}

	if(strcmp(cmd, "/desarmart", true) == 0)
	{
		if(pAdmin[playerid] > 2)
		{
			for(new i = 0; i < MAX_PLAYERS; i++)
			{
				if(IsPlayerConnected(i))
				{
					ResetPlayerWeapons(i);
				}
			}
			format(string, sizeof(string), "O(A) ADM %s (%d) desarmou todos.", GetPlayerNameEx(playerid), playerid);
			SendClientMessageToAll(tcadm, string);
		}
		return 1;
	}

	if(strcmp(cmd, "/amod", true) == 0)
	{
		if(pAdmin[playerid] > 2)
		{
			if(amod[playerid] == 0)
			{
				amod[playerid] = 1;
				SendClientMessage(playerid, 0x0016DDFF, "Mode: Ligado, use /amod novamente para desligar.");
				SendClientMessage(playerid, 0x0016DDFF, "Use as teclas para mover-se: ALT SPACE ENTER Y");
			}
			else if(amod[playerid] == 1)
			{
				amod[playerid] = 0;
				SendClientMessage(playerid, 0x0016DDFF, "Mode: Desligado");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/vercmds", true) == 0)
	{
		if(pAdmin[playerid] > 4)
		{
			if(vercmds[playerid] == 0)
			{
				vercmds[playerid] = 1;
				SendClientMessage(playerid, 0x0016DDFF, "Agora você está lendo os comandos do servidor!");
				SendClientMessage(playerid, Amarelo, "Digite novamente /vercmds para não ver os comandos!");
			}
			else if(vercmds[playerid] == 1)
			{
				vercmds[playerid] = 0;
				SendClientMessage(playerid, 0x0016DDFF, "Agora você não está mais lendo os comandos do servidor!");
				SendClientMessage(playerid, Amarelo, "Digite novamente /vercmds para ver os comandos!");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/verpms", true) == 0)
	{
		if(pAdmin[playerid] > 4)
		{
			if(verpms[playerid] == 0)
			{
				verpms[playerid] = 1;
				SendClientMessage(playerid, 0x0016DDFF, "Agora você está lendo as pms do servidor!");
				SendClientMessage(playerid, Amarelo, "Digite novamente /verpms para não ver as pms!");
			}
			else if(verpms[playerid] == 1)
			{
				verpms[playerid] = 0;
				SendClientMessage(playerid, 0x0016DDFF, "Agora você não está mais lendo as pms do servidor!");
				SendClientMessage(playerid, Amarelo, "Digite novamente /verpms para ver as pms!");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/blockpm", true) == 0)
	{
		if(pAdmin[playerid] > 2)
		{
			if(blockpm[playerid] == 1)
			{
				blockpm[playerid] = 0;
				SendClientMessage(playerid, 0x0016DDFF, "Agora você está recebendo pms!");
				SendClientMessage(playerid, Amarelo, "Digite novamente /blockpm para não receber pms!");
			}
			else if(blockpm[playerid] == 0)
			{
				blockpm[playerid] = 1;
				SendClientMessage(playerid, 0x0016DDFF, "Agora você não está recebendo pms!");
				SendClientMessage(playerid, Amarelo, "Digite novamente /blockpm para receber pms!");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/soltar", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "aAdmin") == 1 || dini_Int(file, "Profissao") == Advogado || dini_Int(file, "Profissao") == Presidente || dini_Int(file, "Profissao") == Prefeito || PlayerInfo[playerid][SCON] == true)
		{
			new plid;

			if(sscanf(cmdtext, "s[7]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "Use: /soltar [id]");
				return 1;
			}
			if(plid == playerid && dini_Int(file, "aAdmin") == 0)
			{
				SendClientMessage(playerid, Vermelho, "Você não pode se soltar, contrate outro advogado.");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				SoltarPlayer(plid);
				SendClientMessage(playerid, COLOR_GREEN, "Jogador solto!");
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "O(A) jogador(a) não está online.");
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não é um advogado.");
		}
		return 1;
	}

	if(NaFaculdade[playerid] == 1) return SendClientMessage(playerid, Vermelho, "Você está na biblioteca e não pode usar nenhum comando.");
	if(preso[playerid] == 1) return SendClientMessage(playerid, Vermelho, "Você não pode usar nenhum comando pois está preso.");
	if(algemado[playerid] == 1) return SendClientMessage(playerid, Vermelho, "Você não pode usar nenhum comando pois está algemado.");
	if(cercado[playerid] == 1) return SendClientMessage(playerid, Vermelho, "Você não pode usar nenhum comando pois está cercado.");

	if(strcmp(cmd, "/games", true) == 0)
	{
		ShowPlayerDialog(playerid, Menugame, DIALOG_STYLE_LIST, "Lan House", "{FF0000}Counter-Striker SA\n{00FF0C}Bomber-Man SA\n{FFFF00}Bate-Bate SA\n{33AAFF}Basquete-Car SA\n{9E3EFF}Monster-Down SA\n{FFFF00}Snake SA", "OK", "Cancelar");
		return 1;
	}

	if(strcmp(cmd, "/sairgame", true) == 0)
	{
		if(nogame[playerid] == 0)
		{
			SendClientMessage(playerid, 0xFF0000AA, "Você não está em um game.");
			return 1;
		}
		nogame[playerid] = 0;
		nobomber[playerid] = 0;

		SetPlayerInterior(playerid, lanI[playerid]);
		SetPlayerPos(playerid, lanX[playerid], lanY[playerid], lanZ[playerid]);

		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		SetPlayerSkin(playerid, dini_Int(file, "Skin"));

		GameTextForPlayer(playerid, "~r~Saiu do Game", 4000, 1);
		SendClientMessage(playerid, 0xFF0000AA, "Você saiu do game.");
		return 1;
	}

	if(strcmp(cmd, "/nalan", true) == 0)
	{
		new count = 0, msg[1000];

		SendClientMessage(playerid, 0x008000AA, "|_Players na Lan House_|");
		for(new i = 0; i < MAX_PLAYERS; i++)
		{
			if(IsPlayerConnected(i) && nogame[i])
			{
				format(msg, sizeof(msg), "%s (ID: %d)", GetPlayerNameEx(i), i);
				SendClientMessage(playerid, 0x0088CAAA, msg);
				count++;
			}
		}
		if(count == 0)
		{
			SendClientMessage(playerid, 0xFF0000AA, "Ninguém está em um Jogo!");
		}
		return 1;
	}

	if(strcmp(cmd, "/naautoescola", true) == 0)
	{
		new count = 0, msg[1000];

		SendClientMessage(playerid, 0x008000AA, "(») Quem está na Auto-Escola («)");
		for(new i = 0; i < MAX_PLAYERS; i++)
		{
			if(IsPlayerConnected(i) && autoescola[i])
			{
				format(msg,sizeof(msg), "%s (ID: %d)", GetPlayerNameEx(i), i);
				SendClientMessage(playerid, 0x0088CAAA, msg);
				count++;
			}
		}
		if(count == 0)
		{
			SendClientMessage(playerid, 0xFF0000AA, "Ninguém está em teste da Auto-Escola!");
		}
		return 1;
	}

	if(strcmp(cmd, "/contar", true) == 0)
	{
		if(pAdmin[playerid] > 0 || nogame[playerid] == 1)
		{
			if(nogame[playerid] == 1)
			{
				for(new i = 0; i < MAX_PLAYERS; i++)
				{
					if(IsPlayerConnected(i))
					{
						if(nogame[i] == 1)
						{
							CreatePlayerCountdown(i, 5, 1);
						}
					}
				}
			}
			else
			{
				CreateCountdown(5, 1);
			}
		}
		return 1;
	}

	if(nogame[playerid] == 1) return SendClientMessage(playerid, Vermelho, "Você não pode usar comando em um game!");
	if(autoescola[playerid] == 1) return SendClientMessage(playerid, Vermelho, "Você não pode usar comando em um teste de Auto-Escola!");

	if(strcmp("/horarios", cmd, true) == 0)
	{
		SendClientMessage(playerid, 0x80FF00AA, "~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~");
		SendClientMessage(playerid, Verde, "A Biblioteca estará aberta nos horários:");
		SendClientMessage(playerid, Amarelo, "» De manha das 9:00 as 12:00");
		SendClientMessage(playerid, Amarelo, "» De tarde das 14:00 as 17:00");
		SendClientMessage(playerid, Amarelo, "» De noite das 21:00 as 24:00");
		SendClientMessage(playerid, Verde, "~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~");
		SendClientMessage(playerid, Verde, "Você receberá seu salário no horário:");
		SendClientMessage(playerid, Amarelo, "» De noite as 19:00 horas!");
		SendClientMessage(playerid, 0x80FF00AA, "~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~");
		return 1;
	}

	if(strcmp(cmd, "/desbugarme", true) == 0)
	{
		#if defined IntroTextUser
		ReconnectPlayer(playerid);
		format(string, sizeof(string), "O(A) %s (%d) tentou se desbugar. ( /desbugarme )", GetPlayerNameEx(playerid), playerid);
		SendClientMessageToAll(tcadm, string);
		#else
		SendClientMessage(playerid, tcadm, "Comando desativado no momento, relogue se estiver bugado!");
		#endif

		return 1;
	}

	if(strcmp("/criarevento", cmd, true) == 0)
	{
		if(pAdmin[playerid] > 0)
		{
			new vagas, nome[64], Float:eXx, Float:eYy, Float:eZz;

			if(sscanf(cmdtext, "s[13]ds[64]", cmd, vagas, nome))
			{
				SendClientMessage(playerid, Vermelho, "/criarevento [vagas] [nome]");
				SendClientMessage(playerid, Amarelo, "Ou opte por usar as Opções de Evento.");

				ClearChatbox(playerid, 3);
				ShowPlayerDialog(playerid, eventos, DIALOG_STYLE_LIST, "Opções de Evento", "{00FFFF}Circuito LS (Normal)\n{FFFF00}Circuito LV (Easy)\n{FF33FF}Corrida Maluca (Normal)\n{00FFFF}Vôo Leve (Normal)\n{FF0000}Vôo Veloz (Expert)\n{003300}Campeonato de Bike (Expert)\n{0000FF}Moto Cross (Expert)", "Criar", "Cancelar");
				return 1;
			}
			GetPlayerPos(playerid, eXx, eYy, eZz);
			PlayerCreateEvent(playerid, vagas, nome, eXx, eYy, eZz, GetPlayerInterior(playerid));
		}
		return 1;
	}

	if(strcmp("/cancelarevento", cmd, true) == 0)
	{
		if(pAdmin[playerid] > 2)
		{
			// Corrida Pista 1
			if(EventoCorrida1 == 1)
			{
				EventoCorrida1 = 0;
				DestroyVehiclesPista1();
				for(new i = 0; i < MAX_PLAYERS; i++)
				{
					if(IsPlayerConnected(i))
					{
						if(InRacePista1[i] == 1)
						{
							InRacePista1[i] = 0;
							DeletePlayerPistaRace1(i);
						}
					}
				}
			}
			// Corrida Pista 2
			if(EventoCorrida2 == 1)
			{
				EventoCorrida2 = 0;
				DestroyVehiclesPista2();
				for(new i = 0; i < MAX_PLAYERS; i++)
				{
					if(IsPlayerConnected(i))
					{
						if(InRacePista2[i] == 1)
						{
							InRacePista2[i] = 0;
							DeletePlayerPistaRace2(i);
						}
					}
				}
			}
			// Corrida Pista 3
			if(EventoCorrida3 == 1)
			{
				EventoCorrida3 = 0;
				DestroyVehiclesPista3();
				for(new i = 0; i < MAX_PLAYERS; i++)
				{
					if(IsPlayerConnected(i))
					{
						if(InRacePista3[i] == 1)
						{
							InRacePista3[i] = 0;
							DeletePlayerPistaRace3(i);
						}
					}
				}
			}
			// Fliping Pista 4
			if(EventoCorrida4 == 1)
			{
				EventoCorrida4 = 0;
				DestroyVehiclesPista4();
				for(new i = 0; i < MAX_PLAYERS; i++)
				{
					if(IsPlayerConnected(i))
					{
						if(InRacePista4[i] == 1)
						{
							InRacePista4[i] = 0;
							DeletePlayerPistaRace4(i);
						}
					}
				}
			}
			// Fliping Pista 5
			if(EventoCorrida5 == 1)
			{
				EventoCorrida5 = 0;
				DestroyVehiclesPista5();
				for(new i = 0; i < MAX_PLAYERS; i++)
				{
					if(IsPlayerConnected(i))
					{
						if(InRacePista5[i] == 1)
						{
							InRacePista5[i] = 0;
							DeletePlayerPistaRace5(i);
						}
					}
				}
			}
			// Campeonato de Bike
			if(EventoCorrida6 == 1)
			{
				EventoCorrida6 = 0;
				DestroyVehiclesPista6();
				for(new i = 0; i < MAX_PLAYERS; i++)
				{
					if(IsPlayerConnected(i))
					{
						if(InRacePista6[i] == 1)
						{
							InRacePista6[i] = 0;
							DeletePlayerPistaRace6(i);
						}
					}
				}
			}
			// Moto Cross
			if(EventoCorrida7 == 1)
			{
				EventoCorrida7 = 0;
				DestroyVehiclesPista7();
				for(new i = 0; i < MAX_PLAYERS; i++)
				{
					if(IsPlayerConnected(i))
					{
						if(InRacePista7[i] == 1)
						{
							InRacePista7[i] = 0;
							DeletePlayerPistaRace7(i);
						}
					}
				}
			}
			PlayerCancelEvent(playerid);
		}
		return 1;
	}

	if(strcmp("/fecharevento", cmd, true) == 0)
	{
		if(pAdmin[playerid] > 0)
		{
			// Corrida Pista 1
			if(EventoCorrida1 == 1)
			{
				EventoCorrida1 = 0;
				DestroyVehiclesPista1();
				for(new i = 0; i < MAX_PLAYERS; i++)
				{
					if(IsPlayerConnected(i))
					{
						if(InRacePista1[i] == 1)
						{
							InRacePista1[i] = 0;
							DeletePlayerPistaRace1(i);
						}
					}
				}
			}
			// Corrida Pista 2
			if(EventoCorrida2 == 1)
			{
				EventoCorrida2 = 0;
				DestroyVehiclesPista2();
				for(new i = 0; i < MAX_PLAYERS; i++)
				{
					if(IsPlayerConnected(i))
					{
						if(InRacePista2[i] == 1)
						{
							InRacePista2[i] = 0;
							DeletePlayerPistaRace2(i);
						}
					}
				}
			}
			// Corrida Pista 3
			if(EventoCorrida3 == 1)
			{
				EventoCorrida3 = 0;
				DestroyVehiclesPista3();
				for(new i = 0; i < MAX_PLAYERS; i++)
				{
					if(IsPlayerConnected(i))
					{
						if(InRacePista3[i] == 1)
						{
							InRacePista3[i] = 0;
							DeletePlayerPistaRace3(i);
						}
					}
				}
			}
			// Fliping Pista 4
			if(EventoCorrida4 == 1)
			{
				EventoCorrida4 = 0;
				DestroyVehiclesPista4();
				for(new i = 0; i < MAX_PLAYERS; i++)
				{
					if(IsPlayerConnected(i))
					{
						if(InRacePista4[i] == 1)
						{
							InRacePista4[i] = 0;
							DeletePlayerPistaRace4(i);
						}
					}
				}
			}
			// Fliping Pista 5
			if(EventoCorrida5 == 1)
			{
				EventoCorrida5 = 0;
				DestroyVehiclesPista5();
				for(new i = 0; i < MAX_PLAYERS; i++)
				{
					if(IsPlayerConnected(i))
					{
						if(InRacePista5[i] == 1)
						{
							InRacePista5[i] = 0;
							DeletePlayerPistaRace5(i);
						}
					}
				}
			}
			// Campeonato de Bike
			if(EventoCorrida6 == 1)
			{
				EventoCorrida6 = 0;
				DestroyVehiclesPista6();
				for(new i = 0; i < MAX_PLAYERS; i++)
				{
					if(IsPlayerConnected(i))
					{
						if(InRacePista6[i] == 1)
						{
							InRacePista6[i] = 0;
							DeletePlayerPistaRace6(i);
						}
					}
				}
			}
			// Moto Cross
			if(EventoCorrida7 == 1)
			{
				EventoCorrida7 = 0;
				DestroyVehiclesPista7();
				for(new i = 0; i < MAX_PLAYERS; i++)
				{
					if(IsPlayerConnected(i))
					{
						if(InRacePista7[i] == 1)
						{
							InRacePista7[i] = 0;
							DeletePlayerPistaRace7(i);
						}
					}
				}
			}
			PlayerCloseEvent(playerid);
		}
		return 1;
	}

	if(strcmp("/expulsarevento", cmd, true) == 0)
	{
		new plid, motivo[64];

		if(sscanf(cmdtext, "s[16]us[64]", cmd, plid, motivo))
		{
			SendClientMessage(playerid, Vermelho, "/expulsarevento [id] [motivo]");
			return 1;
		}
		PlayerExpulsePlayerEvent(playerid, plid, motivo);
		return 1;
	}

	if(strcmp(cmd, "/pausarevento", true) == 0)
	{
		if(EventoCriado == 0)
		{
			SendClientMessage(playerid, Vermelho, "O evento não começou.");
			return 1;
		}
		if(pAdmin[playerid] > 2)
		{
			for(new i = 0; i < MAX_PLAYERS; i++)
			{
				if(IsPlayerConnected(i))
				{
					if(noevento[i] == 1)
					{
						TogglePlayerControllable(i, 0);
					}
				}
			}
			GameTextForAll("~y~Evento ~b~Pausado", 6000, 1);
			format(STRX, sizeof(STRX), "O(A) ADM %s pausou o evento.", GetPlayerNameEx(playerid));
			SendClientMessageToAll(tcadm, STRX);
		}
		return 1;
	}

	if(strcmp(cmd, "/continuarevento", true) == 0)
	{
		if(EventoCriado == 0)
		{
			SendClientMessage(playerid, Vermelho, "O evento não começou.");
			return 1;
		}
		if(pAdmin[playerid] > 2)
		{
			for(new i = 0; i < MAX_PLAYERS; i++)
			{
				if(IsPlayerConnected(i))
				{
					if(noevento[i] == 1)
					{
						TogglePlayerControllable(i, 1);
					}
				}
			}
			GameTextForAll("~y~Evento ~b~Continua", 6000, 1);
			format(STRX, sizeof(STRX), "O(A) ADM %s deu continuação ao evento.", GetPlayerNameEx(playerid));
			SendClientMessageToAll(tcadm, STRX);
		}
		return 1;
	}

	if(strcmp("/comecarevento", cmd, true) == 0)
	{
		if(pAdmin[playerid] > 0)
		{
			// Corrida Pista 1
			if(EventoCorrida1 == 1)
			{
				for(new i = 0; i < MAX_PLAYERS; i++)
				{
					if(IsPlayerConnected(i))
					{
						if(noevento[i] == 1)
						{
							InRacePista1[i] = 1;
							CreatePlayerPistaRace1(i);
							SetTimerEx("DestogglePlayerDynamicRaceCP", 5000, 0, "dd", i, RaceChecksPista1[i][0]);
						}
					}
				}
			}
			// Corrida Pista 2
			if(EventoCorrida2 == 1)
			{
				for(new i = 0; i < MAX_PLAYERS; i++)
				{
					if(IsPlayerConnected(i))
					{
						if(noevento[i] == 1)
						{
							InRacePista2[i] = 1;
							CreatePlayerPistaRace2(i);
							SetTimerEx("DestogglePlayerDynamicRaceCP", 5000, 0, "dd", i, RaceChecksPista2[i][0]);
						}
					}
				}
			}
			// Corrida Pista 3
			if(EventoCorrida3 == 1)
			{
				for(new i = 0; i < MAX_PLAYERS; i++)
				{
					if(IsPlayerConnected(i))
					{
						if(noevento[i] == 1)
						{
							InRacePista3[i] = 1;
							CreatePlayerPistaRace3(i);
							SetTimerEx("DestogglePlayerDynamicRaceCP", 5000, 0, "dd", i, RaceChecksPista3[i][0]);
						}
					}
				}
			}
			// Fliping Pista 4
			if(EventoCorrida4 == 1)
			{
				for(new i = 0; i < MAX_PLAYERS; i++)
				{
					if(IsPlayerConnected(i))
					{
						if(noevento[i] == 1)
						{
							InRacePista4[i] = 1;
							CreatePlayerPistaRace4(i);
							SetTimerEx("DestogglePlayerDynamicRaceCP", 5000, 0, "dd", i, RaceChecksPista4[i][0]);
						}
					}
				}
			}
			// Fliping Pista 5
			if(EventoCorrida5 == 1)
			{
				for(new i = 0; i < MAX_PLAYERS; i++)
				{
					if(IsPlayerConnected(i))
					{
						if(noevento[i] == 1)
						{
							InRacePista5[i] = 1;
							CreatePlayerPistaRace5(i);
							SetTimerEx("DestogglePlayerDynamicRaceCP", 5000, 0, "dd", i, RaceChecksPista5[i][0]);
						}
					}
				}
			}
			// Campeonato de Bike
			if(EventoCorrida6 == 1)
			{
				for(new i = 0; i < MAX_PLAYERS; i++)
				{
					if(IsPlayerConnected(i))
					{
						if(noevento[i] == 1)
						{
							InRacePista6[i] = 1;
							CreatePlayerPistaRace6(i);
							SetTimerEx("DestogglePlayerDynamicRaceCP", 5000, 0, "dd", i, RaceChecksPista6[i][0]);
						}
					}
				}
			}
			// Moto Cross
			if(EventoCorrida7 == 1)
			{
				for(new i = 0; i < MAX_PLAYERS; i++)
				{
					if(IsPlayerConnected(i))
					{
						if(noevento[i] == 1)
						{
							InRacePista7[i] = 1;
							CreatePlayerPistaRace7(i);
							SetTimerEx("DestogglePlayerDynamicRaceCP", 5000, 0, "dd", i, RaceChecksPista7[i][0]);
						}
					}
				}
			}
			PlayerInitEvent(playerid);
		}
		return 1;
	}

	if(strcmp("/irevento", cmd, true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Preso") == 1)
		{
			return SendClientMessage(playerid, Vermelho, "Você está preso!");
		}
		SetPlayerEventPos(playerid);
		return 1;
	}

	if(strcmp("/aerolv", cmd, true) == 0)
	{
		if(IsPlayerInAnyVehicle(playerid))
		{
			SetPlayerInterior(playerid, 0);
			SetVehiclePosEx(GetPlayerVehicleID(playerid), 1285.2111, 1271.6621, 10.5278, 323.3700);
			LinkVehicleToInterior(GetPlayerVehicleID(playerid), 0);
		}
		else
		{
			SetPlayerInterior(playerid, 0);
			SetPlayerPos(playerid, 1285.2111, 1271.6621, 10.5278);
		}
		format(string, sizeof(string), "O(A) jogador(a) %s foi para o AeroLV (») Quer ir? Use: /aerolv", GetPlayerNameEx(playerid));
		SendClientMessageToAll(0x33AA33A, string);
		return 1;
	}

	if(strcmp("/aerosf", cmd, true) == 0)
	{
		if(IsPlayerInAnyVehicle(playerid))
		{
			SetPlayerInterior(playerid, 0);
			SetVehiclePosEx(GetPlayerVehicleID(playerid), -1644.0799, -207.7807, 13.8556, 224.8902);
			LinkVehicleToInterior(GetPlayerVehicleID(playerid), 0);
		}
		else
		{
			SetPlayerInterior(playerid, 0);
			SetPlayerPos(playerid, -1644.0799, -207.7807, 13.8556);
		}
		format(string, sizeof(string), "O(A) jogador(a) %s foi para o AeroSF (») Quer ir? Use: /aerosf", GetPlayerNameEx(playerid));
		SendClientMessageToAll(0x33AA33A, string);
		return 1;
	}

	if(strcmp("/aerols", cmd, true) == 0)
	{
		if(IsPlayerInAnyVehicle(playerid))
		{
			SetPlayerInterior(playerid, 0);
			SetVehiclePosEx(GetPlayerVehicleID(playerid), 1565.4782, -2485.5905, 13.2604, 137.6596);
			LinkVehicleToInterior(GetPlayerVehicleID(playerid), 0);
		}
		else
		{
			SetPlayerInterior(playerid, 0);
			SetPlayerPos(playerid, 1565.4782, -2485.5905, 13.2604);
		}
		format(string, sizeof(string), "O(A) jogador(a) %s foi para o AeroLS (») Quer ir? Use: /aerols", GetPlayerNameEx(playerid));
		SendClientMessageToAll(0x33AA33A, string);
		return 1;
	}

	if(strcmp("/aerovm", cmd, true) == 0)
	{
		if(IsPlayerInAnyVehicle(playerid))
		{
			SetPlayerInterior(playerid, 0);
			SetVehiclePosEx(GetPlayerVehicleID(playerid), 398.7192, 2526.5710, 16.2140, 137.5965);
			LinkVehicleToInterior(GetPlayerVehicleID(playerid), 0);
		}
		else
		{
			SetPlayerInterior(playerid, 0);
			SetPlayerPos(playerid, 398.7192, 2526.5710, 16.2140);
		}
		format(string, sizeof(string), "O(A) jogador(a) %s foi para o AeroVM (») Quer ir? Use: /aerovm", GetPlayerNameEx(playerid));
		SendClientMessageToAll(0x33AA33A, string);
		return 1;
	}

	if(strcmp("/skate", cmd, true) == 0)
	{
		if(IsPlayerInAnyVehicle(playerid))
		{
			SetPlayerInterior(playerid, 0);
			SetVehiclePosEx(GetPlayerVehicleID(playerid), 1877.6309, -1385.1787, 13.2740, 359.2364);
			LinkVehicleToInterior(GetPlayerVehicleID(playerid), 0);
		}
		else
		{
			SetPlayerInterior(playerid, 0);
			SetPlayerPos(playerid, 1877.6309, -1385.1787, 13.2740);
		}
		format(string, sizeof(string), "O(A) jogador(a) %s foi para o Skate (») Quer ir? Use: /skate", GetPlayerNameEx(playerid));
		SendClientMessageToAll(0x33AA33A, string);
		return 1;
	}

	if(strcmp("/grove", cmd, true) == 0)
	{
		if(IsPlayerInAnyVehicle(playerid))
		{
			SetPlayerInterior(playerid, 0);
			SetVehiclePosEx(GetPlayerVehicleID(playerid), 2486.0910, -1657.3436, 13.0549, 85.1661);
			LinkVehicleToInterior(GetPlayerVehicleID(playerid), 0);
		}
		else
		{
			SetPlayerInterior(playerid, 0);
			SetPlayerPos(playerid, 2486.0910, -1657.3436, 13.0549);
		}
		format(string, sizeof(string), "O(A) jogador(a) %s foi para a Grove Street (») Quer ir? Use: /grove", GetPlayerNameEx(playerid));
		SendClientMessageToAll(0x33AA33A, string);
		return 1;
	}

	if(strcmp("/avenidals", cmd, true) == 0)
	{
		if(IsPlayerInAnyVehicle(playerid))
		{
			SetPlayerInterior(playerid, 0);
			SetVehiclePosEx(GetPlayerVehicleID(playerid), 1640.4237, -1034.4143, 61.7462, 170.5807);
			LinkVehicleToInterior(GetPlayerVehicleID(playerid), 0);
		}
		else
		{
			SetPlayerInterior(playerid, 0);
			SetPlayerPos(playerid, 1640.4237, -1034.4143, 61.7462);
		}
		format(string, sizeof(string), "O(A) jogador(a) %s foi para a AvenidaLS (») Quer ir? Use: /avenidals", GetPlayerNameEx(playerid));
		SendClientMessageToAll(0x33AA33A, string);
		return 1;
	}

	if(strcmp("/avenidasf", cmd, true) == 0)
	{
		if(IsPlayerInAnyVehicle(playerid))
		{
			SetPlayerInterior(playerid, 0);
			SetVehiclePosEx(GetPlayerVehicleID(playerid), -1894.1353, -722.6907, 43.3611, 0.4021);
			LinkVehicleToInterior(GetPlayerVehicleID(playerid), 0);
		}
		else
		{
			SetPlayerInterior(playerid, 0);
			SetPlayerPos(playerid, -1894.1353, -722.6907, 43.3611);
		}
		format(string, sizeof(string), "O(A) jogador(a) %s foi para a AvenidaSF (») Quer ir? Use: /avenidasf", GetPlayerNameEx(playerid));
		SendClientMessageToAll(0x33AA33A, string);
		return 1;
	}

	if(strcmp("/avenidalv", cmd, true) == 0)
	{
		if(IsPlayerInAnyVehicle(playerid))
		{
			SetPlayerInterior(playerid, 0);
			SetVehiclePosEx(GetPlayerVehicleID(playerid), 2068.9570, 874.0946, 6.6860, 359.7521);
			LinkVehicleToInterior(GetPlayerVehicleID(playerid), 0);
		}
		else
		{
			SetPlayerInterior(playerid, 0);
			SetPlayerPos(playerid, 2068.9570, 874.0946, 6.6860);
		}
		format(string, sizeof(string), "O(A) jogador(a) %s foi para a AvenidaLV (») Quer ir? Use: /avenidalv", GetPlayerNameEx(playerid));
		SendClientMessageToAll(0x33AA33A, string);
		return 1;
	}

	if(strcmp("/ruafc", cmd, true) == 0)
	{
		if(IsPlayerInAnyVehicle(playerid))
		{
			SetPlayerInterior(playerid, 0);
			SetVehiclePosEx(GetPlayerVehicleID(playerid), -198.2528, 1178.2636, 19.3007, 180.0000);
			LinkVehicleToInterior(GetPlayerVehicleID(playerid), 0);
		}
		else
		{
			SetPlayerInterior(playerid, 0);
			SetPlayerPos(playerid, -198.2528, 1178.2636, 19.3007);
		}
		format(string, sizeof(string), "O(A) jogador(a) %s foi para a RuaFC (») Quer ir? Use: /ruafc", GetPlayerNameEx(playerid));
		SendClientMessageToAll(0x33AA33A, string);
		return 1;
	}

	if(strcmp("/drift", cmd, true) == 0)
	{
		if(IsPlayerInAnyVehicle(playerid))
		{
			SetPlayerInterior(playerid, 0);
			SetVehiclePosEx(GetPlayerVehicleID(playerid), -304.4063, 1500.2224, 75.3148, 184.6666);
			LinkVehicleToInterior(GetPlayerVehicleID(playerid), 0);
		}
		else
		{
			SetPlayerInterior(playerid, 0);
			SetPlayerPos(playerid, -304.4063, 1500.2224, 75.3148);
		}
		format(string, sizeof(string), "O(A) jogador(a) %s foi para o Drift (») Quer ir? Use: /drift", GetPlayerNameEx(playerid));
		SendClientMessageToAll(0x33AA33A, string);
		return 1;
	}

	if(strcmp("/favela", cmd, true) == 0)
	{
		if(IsPlayerInAnyVehicle(playerid))
		{
			SetPlayerInterior(playerid, 0);
			SetVehiclePosEx(GetPlayerVehicleID(playerid), 2172.7041, -1003.7318, 62.5093, 80.3100);
			LinkVehicleToInterior(GetPlayerVehicleID(playerid), 0);
		}
		else
		{
			SetPlayerInterior(playerid, 0);
			SetPlayerPos(playerid, 2172.7041, -1003.7318, 62.5093);
		}
		format(string, sizeof(string), "O(A) jogador(a) %s foi para a Favela (») Quer ir? Use: /favela", GetPlayerNameEx(playerid));
		SendClientMessageToAll(0x33AA33A, string);
		return 1;
	}

	if(strcmp("/lybert", cmd, true) == 0)
	{
		if(IsPlayerInAnyVehicle(playerid))
		{
			SetPlayerInterior(playerid, 1);
			SetVehiclePosEx(GetPlayerVehicleID(playerid), -736.6300, 492.9294, 1371.6842, 149.0222);
			LinkVehicleToInterior(GetPlayerVehicleID(playerid), 1);
		}
		else
		{
			SetPlayerInterior(playerid, 1);
			SetPlayerPos(playerid, -736.6300, 492.9294, 1371.6842);
		}
		format(string, sizeof(string), "O(A) jogador(a) %s foi para Lybert City (») Quer ir? Use: /lybert", GetPlayerNameEx(playerid));
		SendClientMessageToAll(0x33AA33A, string);
		return 1;
	}

	if(strcmp("/detran", cmd, true) == 0)
	{
		if(IsPlayerInAnyVehicle(playerid))
		{
			SetPlayerInterior(playerid, 0);
			SetVehiclePosEx(GetPlayerVehicleID(playerid), 1082.6553, 1375.7139, 10.3849, 89.6340);
			LinkVehicleToInterior(GetPlayerVehicleID(playerid), 0);
		}
		else
		{
			SetPlayerInterior(playerid, 0);
			SetPlayerPos(playerid, 1082.6553, 1375.7139, 10.3849);
		}
		format(string, sizeof(string), "O(A) jogador(a) %s foi para a Detran (») Quer ir? Use: /detran", GetPlayerNameEx(playerid));
		SendClientMessageToAll(0x33AA33A, string);
		return 1;
	}

	if(strcmp("/bay", cmd, true) == 0)
	{
		if(IsPlayerInAnyVehicle(playerid))
		{
			SetPlayerInterior(playerid, 0);
			SetVehiclePosEx(GetPlayerVehicleID(playerid), -2685.7927,1274.2789,55.4297, 89.6340);
			LinkVehicleToInterior(GetPlayerVehicleID(playerid), 0);
		}
		else
		{
			SetPlayerInterior(playerid, 0);
			SetPlayerPos(playerid, -2685.7927,1274.2789,55.4297);
		}
		format(string, sizeof(string), "O(A) jogador(a) %s foi para a Bayside (») Quer ir? Use: /bay", GetPlayerNameEx(playerid));
		SendClientMessageToAll(0x33AA33A, string);
		return 1;
	}

	if(strcmp("/shop", cmd, true) == 0)
 	{
		SetPlayerInterior(playerid, 0);
		SetPlayerPos(playerid, 1126.28, -1484.79, 16.50);
		
		format(string, sizeof(string), "O(A) jogador(a) %s foi ao shopping (») Quer ir? Use: /shop", GetPlayerNameEx(playerid));
		SendClientMessageToAll(0x33AA33A, string);
		return 1;
	}
	if(strcmp("/arenadm", cmd, true) == 0)
	{
		SetPlayerInterior(playerid, 0);
		SetPlayerPos(playerid, 1369.7585, 2195.4555, 9.7578);

		format(string, sizeof(string), "O(A) jogador(a) %s foi para a ArenaDM (») Quer ir? Use: /arenadm", GetPlayerNameEx(playerid));
		SendClientMessageToAll(0x33AA33A, string);
		return 1;
	}

	if(strcmp("/oceandm", cmd, true) == 0)
	{
		SetPlayerInterior(playerid, 0);
		SetPlayerPos(playerid, 2734.7502, -2450.1545, 17.5937);

		format(string, sizeof(string), "O(A) jogador(a) %s foi para a OceanDM (») Quer ir? Use: /oceandm", GetPlayerNameEx(playerid));
		SendClientMessageToAll(0x33AA33A, string);
		return 1;
	}

	if(strcmp("/swatdm", cmd, true) == 0)
	{
		SetPlayerInterior(playerid, 0);
		SetPlayerPos(playerid, 1182.0980, -2036.8110, 69.0078);

		format(string, sizeof(string), "O(A) jogador(a) %s foi para a SwatDM (») Quer ir? Use: /swatdm", GetPlayerNameEx(playerid));
		SendClientMessageToAll(0x33AA33A, string);
		return 1;
	}

	if(strcmp("/naviodm", cmd, true) == 0)
	{
		SetPlayerInterior(playerid, 0);
		SetPlayerPos(playerid, -2373.2142, 1551.4996, 2.1172);

		format(string, sizeof(string), "O(A) jogador(a) %s foi para o NavioDM (») Quer ir? Use: /naviodm", GetPlayerNameEx(playerid));
		SendClientMessageToAll(0x33AA33A, string);
		return 1;
	}

	if(strcmp("/a51", cmd, true) == 0)
	{
		SetPlayerInterior(playerid, 0);
		SetPlayerPos(playerid, 88.3497, 1918.6447, 17.8780);

		format(string, sizeof(string), "O(A) jogador(a) %s foi para a Area51 (») Quer ir? Use: /a51", GetPlayerNameEx(playerid));
		SendClientMessageToAll(0x33AA33A, string);
		return 1;
	}

	if(strcmp("/ufo", cmd, true) == 0)
	{
		SetPlayerInterior(playerid, 0);
		SetPlayerPos(playerid, 1931.7877, -506.3997, 21.1385);

		format(string, sizeof(string), "O(A) jogador(a) %s foi para o Ufo (») Quer ir? Use: /ufo", GetPlayerNameEx(playerid));
		SendClientMessageToAll(0x33AA33A, string);
		return 1;
	}

	if(strcmp("/montesf", cmd, true) == 0)
	{
		SetPlayerInterior(playerid, 0);
		SetPlayerPos(playerid, -2233.7698, -1736.4923, 480.8207);

		format(string, sizeof(string), "O(A) jogador(a) %s foi para o MonteSF (») Quer ir? Use: /montesf", GetPlayerNameEx(playerid));
		SendClientMessageToAll(0x33AA33A, string);
		return 1;
	}

	if(strcmp("/cassino", cmd, true) == 0)
	{
		SetPlayerInterior(playerid, 0);
		SetPlayerPos(playerid, 2022.4974, 1007.9083, 10.8203);

		format(string, sizeof(string), "O(A) jogador(a) %s foi para o Cassino (») Quer ir? Use: /cassino", GetPlayerNameEx(playerid));
		SendClientMessageToAll(0x33AA33A, string);
		return 1;
	}

	if(strcmp("/pref", cmd, true) == 0)
	{
		SetPlayerInterior(playerid, 0);
		SetPlayerPos(playerid, 1481.2938, -1769.7637, 18.7958);

		format(string, sizeof(string), "O(A) jogador(a) %s foi para a Prefeitura (») Quer ir? Use: /pref", GetPlayerNameEx(playerid));
		SendClientMessageToAll(0x33AA33A, string);
		return 1;
		}
	if(strcmp("/acc", cmd, true) == 0)
	{
	    SetPlayerInterior(playerid, 0);
	    SetPlayerPos(playerid, 2088.5125, 2224.2617, 11.0234);
		return 1;
	}
	if(strcmp("/cag", cmd, true) == 0)
	{
		ApplyAnimation(playerid,"FOOD","FF_Dam_Fwd",4.1,0,1,1,1,1);
		return 1;
	}
	if(strcmp("/postols", cmd, true) == 0)
	{
		if(IsPlayerInAnyVehicle(playerid))
		{
			SetPlayerInterior(playerid, 0);
			SetVehiclePosEx(GetPlayerVehicleID(playerid), 1938.5044,-1772.0070,13.0078, 0.4021);
			LinkVehicleToInterior(GetPlayerVehicleID(playerid), 0);
		}
		else
		{
			SetPlayerInterior(playerid, 0);
			SetPlayerPos(playerid, 1938.5044,-1772.0070,13.0078);
		}
		format(string, sizeof(string), "O(A) jogador(a) %s foi abastecer o carro no posto de Los Santos (») Quer ir? Use: /postols", GetPlayerNameEx(playerid));
		SendClientMessageToAll(0x33AA33A, string);
		return 1;
	}

	if(strcmp("/postolv", cmd, true) == 0)
	{
		if(IsPlayerInAnyVehicle(playerid))
		{
			SetPlayerInterior(playerid, 0);
			SetVehiclePosEx(GetPlayerVehicleID(playerid), 2638.9553, 1107.1788, 10.8203, 0.4021);
			LinkVehicleToInterior(GetPlayerVehicleID(playerid), 0);
		}
		else
		{
			SetPlayerInterior(playerid, 0);
			SetPlayerPos(playerid, 2638.9553, 1107.1788, 10.8203);
		}
		format(string, sizeof(string), "O(A) jogador(a) %s foi abastecer o carro no posto de Las Venturas (») Quer ir? Use: /postolv", GetPlayerNameEx(playerid));
		SendClientMessageToAll(0x33AA33A, string);
		return 1;
	}

	if(strcmp("/postosf", cmd, true) == 0)
	{
		if(IsPlayerInAnyVehicle(playerid))
		{
			SetPlayerInterior(playerid, 0);
			SetVehiclePosEx(GetPlayerVehicleID(playerid), 1380.6123, 456.5110, 19.8894, 0.4021);
			LinkVehicleToInterior(GetPlayerVehicleID(playerid), 0);
		}
		else
		{
			SetPlayerInterior(playerid, 0);
			SetPlayerPos(playerid, 1380.6123,456.5110,19.8894);
		}
		format(string, sizeof(string), "O(A) jogador(a) %s foi abastecer o carro no posto de San Fierro (») Quer ir? Use: /postosf", GetPlayerNameEx(playerid));
		SendClientMessageToAll(0x33AA33A, string);
		return 1;
	}

	if(strcmp("/dp", cmd, true) == 0)
	{
		SetPlayerInterior(playerid, 0);
		SetPlayerPos(playerid, 1553.7681, -1675.6943, 16.1953);

		format(string, sizeof(string), "O(A) jogador(a) %s foi para a Delegacia (») Quer ir? Use: /dp", GetPlayerNameEx(playerid));
		SendClientMessageToAll(0x33AA33A, string);
		return 1;
	}

	if(strcmp("/bi", cmd, true) == 0)
	{
		SetPlayerInterior(playerid, 0);
		SetPlayerPos(playerid, 1081.0706, -1701.3231, 13.5469);

		format(string, sizeof(string), "O(A) jogador(a) %s foi para a Biblioteca (») Quer ir? Use: /bi", GetPlayerNameEx(playerid));
		SendClientMessageToAll(0x33AA33A, string);
		return 1;
	}

	if(strcmp("/facul", cmd, true) == 0)
	{
		SetPlayerInterior(playerid, 0);
		SetPlayerPos(playerid, 1225.5422, -1844.4948, 13.5468);

		format(string, sizeof(string), "O(A) jogador(a) %s foi para a Faculdade (») Quer ir? Use: /facul", GetPlayerNameEx(playerid));
		SendClientMessageToAll(0x33AA33A, string);
		return 1;
	}

	if(strcmp("/ilhap", cmd, true) == 0)
	{
		SetPlayerInterior(playerid, 0);
		SetPlayerPos(playerid, 794.1492, -3235.0273, 9.7802);

		TogglePlayerControllable(playerid, false);
		SetTimerEx("DestogglePlayerControllable", 1000, 0, "d", playerid);

		format(string, sizeof(string), "O(A) jogador(a) %s foi para a Ilha Pirata (») Quer ir? Use: /ilhap", GetPlayerNameEx(playerid));
		SendClientMessageToAll(0x33AA33A, string);
		return 1;
	}

	if(strcmp("/lostc", cmd, true) == 0)
	{
		SetPlayerInterior(playerid, 0);
		SetPlayerPos(playerid, 2188.6123, 1038.7008, 494.3419);

		TogglePlayerControllable(playerid, false);
		SetTimerEx("DestogglePlayerControllable", 1000, 0, "d", playerid);

		format(string, sizeof(string), "O(A) jogador(a) %s foi para a Lost City (») Quer ir? Use: /lostc", GetPlayerNameEx(playerid));
		SendClientMessageToAll(0x33AA33A, string);
		return 1;
	}

	if(strcmp("/jump", cmd, true) == 0)
	{
		SetPlayerInterior(playerid, 0);
		SetPlayerPos(playerid, 2072.8071, 3.2631, 300.6766);

		TogglePlayerControllable(playerid, false);
		SetTimerEx("JumpDesbug", 3000, 0, "d", playerid);

		format(string, sizeof(string), "O(A) jogador(a) %s foi para o Jump (») Quer ir? Use: /jump", GetPlayerNameEx(playerid));
		SendClientMessageToAll(0x33AA33A, string);
		return 1;
	}

	if(strcmp("/drop", cmd, true) == 0)
	{
		SetPlayerInterior(playerid, 0);
		SetPlayerPos(playerid, 1006.9839, -2623.7243, 149.2153);

		TogglePlayerControllable(playerid, false);
		SetTimerEx("DestogglePlayerControllable", 1000, 0, "d", playerid);

		format(string, sizeof(string), "O(A) jogador(a) %s foi para o Drop (») Quer ir? Use: /drop", GetPlayerNameEx(playerid));
		SendClientMessageToAll(0x33AA33A, string);
		return 1;
	}

	if(strcmp("/teles", cmd, true) == 0)
	{
		SendClientMessage(playerid, 0x33AA33A, "=-=-=-=-=-=-=-=-=-=-=-= Teleportes =-=-=-=-=-=-=-=-=-=-=-=");
		SendClientMessage(playerid, 0x33AAFFFF, "/avenidalv /avenidasf /avenidals /ruafc /drift /lybert");
		SendClientMessage(playerid, 0x33AAFFFF, "/aerolv /aerosf /aerols /aerovm /skate /grove /jump");
		SendClientMessage(playerid, 0x33AAFFFF, "/pref /dp /bi /cassino /ufo /montesf /ilhap /a51 /drop");
		SendClientMessage(playerid, 0x33AAFFFF, "/favela /arenadm /oceandm /swatdm /naviodm /lostc /facul");
		SendClientMessage(playerid, 0x33AA33A, "=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=");
		return 1;
	}

	if(strcmp("/evento", cmd, true) == 0)
	{
		if(pAdmin[playerid] > 0)
		{
			SendClientMessage(playerid, 0x008000AA, "Use: /criarevento para criar um evento");
			SendClientMessage(playerid, 0x008000AA, "Use: /expulsarevento para expulsar alguem do evento");
			SendClientMessage(playerid, 0x008000AA, "Use: /comecarevento para iniciar o evento");
			SendClientMessage(playerid, 0x008000AA, "Use: /pausarevento para pausar o evento");
			SendClientMessage(playerid, 0x008000AA, "Use: /continuarevento para continuar o evento em pausa");
			SendClientMessage(playerid, 0x008000AA, "Use: /cancelarevento para cancelar o evento");
			SendClientMessage(playerid, 0x008000AA, "Use: /fecharevento quando o evento começado acabar");
		}
		return 1;
	}

	if(strcmp(cmd, "/comandoscelular", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Celular") == 1)
		{
			SendClientMessage(playerid, Verde, "-- Menu celular --.");
			SendClientMessage(playerid, Amarelo, "/ligar [ID] para ligar.");
			SendClientMessage(playerid, Amarelo, "/atender para atender.");
			SendClientMessage(playerid, Amarelo, "/ignorar para não atender.");
			SendClientMessage(playerid, Amarelo, "/desligar para desligar.");
		}
		else
		{
			SendClientMessage(playerid, Vermelho, " Você precisa de um celular.");
		}
		return 1;
	}

	if(!strcmp(cmd, "/ligar", true))
	{
		if(sscanf(cmdtext, "s[7]u", cmd, id2))
		{
			SendClientMessage(playerid, RED, "Use /ligar [id]");
			return 1;
		}

		docommand = playerid;

		if(!IsPlayerConnected(id2))
		{
			SendClientMessage(docommand, RED, "Fora de área, tente novamente mais tarde.");
			return 1;
		}
		if(InCall[id2] == 1)
		{
			SendClientMessage(gc, RED, "O telefone está ocupado, tente novamente mais tarde.");
			return 1;
		}
		if(InCall[playerid] == 1)
		{
			SendClientMessage(gc, RED, "Você já está em uma chamada.");
			return 1;
		}
		if(GetPlayerState(docommand) == PLAYER_STATE_DRIVER)
		{
			SendClientMessage(docommand, RED, "Você não pode dirigir e falar no telefone ao mesmo tempo.");
			return 1;
		}
		if(IsPlayerNPC(id2))
		{
			SendClientMessage(docommand, RED, "Este telefone não está recebendo chamadas.");
			return 1;
		}
		if(id2 == playerid)
		{
			SendClientMessage(docommand, RED, "Você não pode ligar para si mesmo.");
			return 1;
		}
		if(IsPlayerInAnyVehicle(id2) || IsPlayerInAnyVehicle(playerid))
		{
			SendClientMessage(playerid, Vermelho, " A pessoa que você ligou está dentro de um carro.");
			return 1;
		}
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Celular") == 1)
		{
			format(file2, sizeof(file2), PASTA_CONTAS, GetPlayerNameEx(id2));
			if(dini_Int(file2, "Celular") == 1)
			{
				InCall[docommand] = 0;
				InCall[id2] = 0;

				GetCall[docommand] = 0;
				GetCall[id2] = 1;

				format(str3, sizeof(str3), "Você está ligando para %s, aguarde ele(a) atender. tammm... tammm...", GetPlayerNameEx(id2));
				SendClientMessage(docommand, WHITE, str3);

				format(str3, sizeof(str3), "O(A) jogador(a) %s está ligando para você. trim... trim... trim...", GetPlayerNameEx(docommand));
				SendClientMessage(id2, WHITE, str3);

				SendClientMessage(id2, BLUEWHITE, "Para atender o telefone use /atender e para recusar a chamada use /ignorar");
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Este telefone não está disponível no momento!");
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem um telefone!");
		}
		return 1;
	}

	gc = id2;
	sender = docommand;

	if(!strcmp(cmd, "/ignorar", true))
	{
		if(GetCall[playerid] == 0)
		{
			SendClientMessage(playerid, RED, "Você não está recebendo uma chamada.");
			return 1;
		}
		InCall[sender] = 0;
		InCall[gc] = 0;

		GetCall[sender] = 0;
		GetCall[gc] = 0;

		format(str3, sizeof(str3), "Você não atendeu a chamada de %s.", GetPlayerNameEx(sender));
		SendClientMessage(gc, GREEN, str3);

		format(str3, sizeof(str3), "O player %s não atendeu o telefone.", GetPlayerNameEx(gc));
		SendClientMessage(sender, GRAY, str3);
		return 1;
	}

	if(!strcmp(cmd, "/atender", true))
	{
		if(IsPlayerInAnyVehicle(playerid))
		{
			SendClientMessage(playerid, Vermelho, "Você está dentro de um carro e não pode atender.");
			return 1;
		}
		if(GetCall[playerid] == 0)
		{
			return SendClientMessage(playerid, RED, "Você não está recebendo uma chamada.");
		}
		if(InCall[playerid] == 1)
		{
			return SendClientMessage(playerid, RED, "Seu telefone está tocando. Plinnnn... Plinnnn... Plinnnn...");
		}
		if(GetPlayerState(playerid) == PLAYER_STATE_DRIVER)
		{
			return SendClientMessage(playerid, RED, "Você não pode dirigir e falar no celular ao mesmo tempo.");
		}
		InCall[sender] = 1;
		InCall[gc] = 1;

		GetCall[sender] = 0;
		GetCall[gc] = 0;

		SetPlayerSpecialAction(sender, SPECIAL_ACTION_USECELLPHONE);
		SetPlayerSpecialAction(gc, SPECIAL_ACTION_USECELLPHONE);

		format(str3, sizeof(str3), "Você atendeu o telefonema de %s.", GetPlayerNameEx(sender));
		SendClientMessage(gc, GREEN, str3);

		format(str3, sizeof(str3), " O(A) jogador(a) %s atendeu o telefone.", GetPlayerNameEx(gc));
		SendClientMessage(sender, GREEN, str3);
		return 1;
	}

	if(!strcmp(cmd, "/desligar", true))
	{
		if(IsPlayerInAnyVehicle(playerid))
		{
			SendClientMessage(playerid, Vermelho, "Você está dentro de um carro e não pode desligar.");
			return 1;
		}
		if(InCall[gc] == 0)
		{
			SendClientMessage(gc, RED, "Você não está em uma chamada agora.");
			return 1;
		}
		if(gc == playerid)
		{
			InCall[gc] = 0;
			GetCall[gc] = 0;

			InCall[sender] = 0;
			GetCall[sender] = 0;

			SetPlayerSpecialAction(gc, SPECIAL_ACTION_STOPUSECELLPHONE);
			SetPlayerSpecialAction(sender, SPECIAL_ACTION_STOPUSECELLPHONE);

			format(str3, sizeof(str3), "Você finalizou a chamada com %s.", GetPlayerNameEx(sender));
			SendClientMessage(gc, GREEN, str3);

			format(str3, sizeof(str3), "%s desligou o telefone.", GetPlayerNameEx(gc));
			SendClientMessage(sender, GRAY, str3);
		}
		if(sender == playerid)
		{
			InCall[gc] = 0;
			GetCall[gc] = 0;

			InCall[sender] = 0;
			GetCall[sender] = 0;

			SetPlayerSpecialAction(gc, SPECIAL_ACTION_STOPUSECELLPHONE);
			SetPlayerSpecialAction(sender, SPECIAL_ACTION_STOPUSECELLPHONE);

			format(str3, sizeof(str3), "Você finalizou a chamada com %s.", GetPlayerNameEx(gc));
			SendClientMessage(sender, GREEN, str3);

			format(str3, sizeof(str3), "%s desligou o telefone.", GetPlayerNameEx(sender));
			SendClientMessage(gc, GRAY, str3);
		}
		return 1;
	}

	if(strcmp(cmd, "/continuar", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Continuar") == 0)
		{
			SendClientMessage(playerid, Vermelho, "Você não tem nova posição salva.");
			SendClientMessage(playerid, Vermelho, "Use /salvar para salvar uma posição.");
		}
		else
		{
			if(IsPlayerInAnyVehicle(playerid))
			{
				SendClientMessage(playerid, Vermelho, "Você está dentro de um carro.");
			}
			else
			{
				dini_IntSet(file, "Continuar", 0);
				SetPlayerInterior(playerid, dini_Int(file, "ContinuarI"));
				SetPlayerPos(playerid, dini_Int(file, "ContinuarX"), dini_Int(file, "ContinuarY"), dini_Int(file, "ContinuarZ"));

				SendClientMessage(playerid, Azul, "Você foi até sua posição salva.");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/veiculosa", true) == 0)
	{
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos A ~~~~~~~~~~~~~~~~~~~~~~~~");
		SendClientMessage(playerid, COLOR_WHITE, " AT-400: 577 || Andromada: 592");
		SendClientMessage(playerid, COLOR_WHITE, " Admiral: 445 || Alpha: 602 || Ambulan: 416");
		SendClientMessage(playerid, COLOR_WHITE, " Artict1: 435 || Artict2: 450");
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos A ~~~~~~~~~~~~~~~~~~~~~~~~");
		return 1;
	}

	if(strcmp(cmd, "/veiculosb", true) == 0)
	{
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos B ~~~~~~~~~~~~~~~~~~~~~~~~");
		SendClientMessage(playerid, COLOR_WHITE, " BF-400: 581 || Beagle: 511 || Baggage: 485 || Bandito: 568");
		SendClientMessage(playerid, COLOR_WHITE, " Banshee: 429 || Barracks: 433 || Benson: 499 || Bfinject: 424");
		SendClientMessage(playerid, COLOR_WHITE, " Blade: 536 || Blistac: 496 || Bloodra: 504 || Bobcat: 422");
		SendClientMessage(playerid, COLOR_WHITE, " Boxburg: 609 || Boxville: 498 || Bravura: 401 || Broadway: 575");
		SendClientMessage(playerid, COLOR_WHITE, " Buccanee: 518 || Buffalo: 402 || Bullet: 541 || Bagboxb: 607");
		SendClientMessage(playerid, COLOR_WHITE, " Burrito: 482 || Bus: 431 || Bike: 509 || BMX: 481 || Bagboxa: 606");
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos B ~~~~~~~~~~~~~~~~~~~~~~~~");
		return 1;
	}

	if(strcmp(cmd, "/veiculosc", true) == 0)
	{
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos C ~~~~~~~~~~~~~~~~~~~~~~~~");
		SendClientMessage(playerid, COLOR_WHITE, " CopBike: 523 || Cropdust: 512 || CoastGuard: 472");
		SendClientMessage(playerid, COLOR_WHITE, " Caddy: 457 || Cadrona: 527 || Camper: 483 || Cement: 524");
		SendClientMessage(playerid, COLOR_WHITE, " Cheetah: 415 || Clover: 542 || Club: 589 || Coach: 437");
		SendClientMessage(playerid, COLOR_WHITE, " Combine: 532 || Comet: 480 || CopCarLS: 596 || CopCar: 599");
		SendClientMessage(playerid, COLOR_WHITE, " CopCarSF: 597 || CopCarLV: 598 || Cft30: 578 || Cozer: 486");
		SendClientMessage(playerid, COLOR_WHITE, " Cargobob: 548 || Cabbie: 438");
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos C ~~~~~~~~~~~~~~~~~~~~~~~~");
		return 1;
	}

	if(strcmp(cmd, "/veiculosd", true) == 0)
	{
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos D ~~~~~~~~~~~~~~~~~~~~~~~~");
		SendClientMessage(playerid, COLOR_WHITE, " Dodo: 593 || Dinghy: 473 || Dumper: 406 || Duneride: 573");
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos D ~~~~~~~~~~~~~~~~~~~~~~~~");
		return 1;
	}

	if(strcmp(cmd, "/veiculose", true) == 0)
	{
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos E ~~~~~~~~~~~~~~~~~~~~~~~~");
		SendClientMessage(playerid, COLOR_WHITE, " Elegant: 507 || Elegy: 562 || Emperor: 585");
		SendClientMessage(playerid, COLOR_WHITE, " Esperant: 419 || Euros: 587 || Enforcer: 427");
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos E ~~~~~~~~~~~~~~~~~~~~~~~~");
		return 1;
	}

	if(strcmp(cmd, "/veiculosf", true) == 0)
	{
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos F ~~~~~~~~~~~~~~~~~~~~~~~~");
		SendClientMessage(playerid, COLOR_WHITE, " Faggio: 462 || FCR-900: 521 || Freeway: 463 || Fbiranch: 490");
		SendClientMessage(playerid, COLOR_WHITE, " Fbitruck: 528 || Feltze: 533 || Firela: 544 || Firetruck: 407");
		SendClientMessage(playerid, COLOR_WHITE, " Flash: 565 || Flatbed: 455 || Forklift: 530 || Fortune: 526");
		SendClientMessage(playerid, COLOR_WHITE, " Freight: 537 || Farmtr1: 610 ");
		SendClientMessage(playerid, verdel,"~~~~~~~~~~~~~~~~~~~~~~~~ Veículos F ~~~~~~~~~~~~~~~~~~~~~~~~");
		return 1;
	}

	if(strcmp(cmd, "/veiculosg", true) == 0)
	{
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos G ~~~~~~~~~~~~~~~~~~~~~~~~");
		SendClientMessage(playerid, COLOR_WHITE, " Glendale: 466 || Glenshit: 604 || Greenwoo: 492 ");
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos G ~~~~~~~~~~~~~~~~~~~~~~~~");
		return 1;
	}

	if(strcmp(cmd, "/veiculosh", true) == 0)
	{
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos H ~~~~~~~~~~~~~~~~~~~~~~~~");
		SendClientMessage(playerid, COLOR_WHITE, " Hydra: 520 || Hermes: 474 || Hotdog: 588 ");
		SendClientMessage(playerid, COLOR_WHITE, " Hotrina: 502 || Hotrinb: 503 || Hotring: 594 ");
		SendClientMessage(playerid, COLOR_WHITE, " Hustler: 545 || Huntley: 579 || Hotknife: 434");
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos H ~~~~~~~~~~~~~~~~~~~~~~~~");
		return 1;
	}

	if(strcmp(cmd, "/veiculosi", true) == 0)
	{
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos I ~~~~~~~~~~~~~~~~~~~~~~~~");
		SendClientMessage(playerid, COLOR_WHITE, " Infernus: 411 || Intruder: 546");
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos I ~~~~~~~~~~~~~~~~~~~~~~~~");
		return 1;
	}

	if(strcmp(cmd, "/veiculosj", true) == 0)
	{
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos J ~~~~~~~~~~~~~~~~~~~~~~~~");
		SendClientMessage(playerid, COLOR_WHITE, " Jester: 559 || Journey: 508 ");
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos J ~~~~~~~~~~~~~~~~~~~~~~~~");
		return 1;
	}

	if(strcmp(cmd, "/veiculosk", true) == 0)
	{
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos K ~~~~~~~~~~~~~~~~~~~~~~~~");
		SendClientMessage(playerid, COLOR_WHITE, " Kart: 571");
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos K ~~~~~~~~~~~~~~~~~~~~~~~~");
		return 1;
	}

	if(strcmp(cmd, "/veiculosl", true) == 0)
	{
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos L ~~~~~~~~~~~~~~~~~~~~~~~~");
		SendClientMessage(playerid, COLOR_WHITE, " Launch: 595 || Landstal: 400");
		SendClientMessage(playerid, COLOR_WHITE, " Leviathn: 417 || Linerun: 403");
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos L ~~~~~~~~~~~~~~~~~~~~~~~~");
		return 1;
	}

	if(strcmp(cmd, "/veiculosm", true) == 0)
	{
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos M ~~~~~~~~~~~~~~~~~~~~~~~~");
		SendClientMessage(playerid, COLOR_WHITE, " Marquis: 484 || Majestic: 517 || Manana: 410 ");
		SendClientMessage(playerid, COLOR_WHITE, " Merit: 551 || Mesa: 500 || Moonbeam: 418 || Mowerr: 572");
		SendClientMessage(playerid, COLOR_WHITE, " Mrwhoop: 423 || Mule: 414 || Monster: 444 || MonsterA: 556");
		SendClientMessage(playerid, COLOR_WHITE, " MonsterB: 557 || Mountain Bike: 510 || Maverick: 487");
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos M ~~~~~~~~~~~~~~~~~~~~~~~~");
		return 1;
	}

	if(strcmp(cmd, "/veiculosn", true) == 0)
	{
		SendClientMessage(playerid, verdel,"~~~~~~~~~~~~~~~~~~~~~~~~ Veículos N ~~~~~~~~~~~~~~~~~~~~~~~~");
		SendClientMessage(playerid, COLOR_WHITE," NRG-500: 522 || Nevada: 553 || Nebula: 516 || Newsvan: 582");
		SendClientMessage(playerid, verdel,"~~~~~~~~~~~~~~~~~~~~~~~~ Veículos N ~~~~~~~~~~~~~~~~~~~~~~~~");
		return 1;
	}

	if(strcmp(cmd, "/veiculoso", true) == 0)
	{
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos O ~~~~~~~~~~~~~~~~~~~~~~~~");
		SendClientMessage(playerid, COLOR_WHITE, " Oceanic: 467");
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos O ~~~~~~~~~~~~~~~~~~~~~~~~");
		return 1;
	}

	if(strcmp(cmd, "/veiculosp", true) == 0)
	{
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos P ~~~~~~~~~~~~~~~~~~~~~~~~");
		SendClientMessage(playerid, COLOR_WHITE, " PCJ-600: 461 || Pizzaboy: 448 || Predator: 430");
		SendClientMessage(playerid, COLOR_WHITE, " Packer: 443 || Patriot: 470 || Peren: 404 || Petro: 514");
		SendClientMessage(playerid, COLOR_WHITE, " Phoenix: 603 || Picador: 600 || Pony: 413 || Premier: 426");
		SendClientMessage(playerid, COLOR_WHITE, " Previon: 436 || Primo: 547 || Polmav: 497 || Petrotr: 584");
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos P ~~~~~~~~~~~~~~~~~~~~~~~~");
		return 1;
	}

	if(strcmp(cmd, "/veiculosq", true) == 0)
	{
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos Q ~~~~~~~~~~~~~~~~~~~~~~~~");
		SendClientMessage(playerid, COLOR_WHITE, " Quad: 471");
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos Q ~~~~~~~~~~~~~~~~~~~~~~~~");
		return 1;
	}

	if(strcmp(cmd, "/veiculosr", true) == 0)
	{
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos R ~~~~~~~~~~~~~~~~~~~~~~~~");
		SendClientMessage(playerid, COLOR_WHITE, " Rustler: 476 || RC Barron: 464 || Reefer: 453");
		SendClientMessage(playerid, COLOR_WHITE, " Rancher: 489 || Rcbandit: 441 || Rccam: 594 ");
		SendClientMessage(playerid, COLOR_WHITE, " Rctiger: 564 || Rdtrain: 515 || Regina: 479 ");
		SendClientMessage(playerid, COLOR_WHITE, " Remingtn: 534 || Rhino: 432 || Rnchlure: 505 || Rcraider: 465");
		SendClientMessage(playerid, COLOR_WHITE, " Romero: 442 || Rumpo: 440 || Raindanc: 563 || Rcgoblin: 501");
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos R ~~~~~~~~~~~~~~~~~~~~~~~~");
		return 1;
	}

	if(strcmp(cmd, "/veiculoss", true) == 0)
	{
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos S ~~~~~~~~~~~~~~~~~~~~~~~~");
		SendClientMessage(playerid, COLOR_WHITE, " Sanchez: 468 || Stuntplane: 513 || Skimmer: 460 || Sparrow: 469");
		SendClientMessage(playerid, COLOR_WHITE, " Shamal: 519 || Speeder: 452 || Squalo: 446 || Sabre: 475");
		SendClientMessage(playerid, COLOR_WHITE, " Sadler: 543 || Sadlshit: 605 || Sandking: 495 || Savanna: 567");
		SendClientMessage(playerid, COLOR_WHITE, " Securica: 428 || Sentinel: 405 || Slamvan: 535 || Solair: 458");
		SendClientMessage(playerid, COLOR_WHITE, " Stafford: 580 || Stallion: 439 || Stratum: 561 || Stretch: 409 ");
		SendClientMessage(playerid, COLOR_WHITE, " Sultan: 560 || Sunrise: 550 || Supergt: 506 || Swatvan: 601 ");
		SendClientMessage(playerid, COLOR_WHITE, " Sweeper: 574 || Streak: 538 || Streakc: 570 || Seasparr: 447");
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos S ~~~~~~~~~~~~~~~~~~~~~~~~");
		return 1;
	}

	if(strcmp(cmd, "/veiculost", true) == 0)
	{
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos T ~~~~~~~~~~~~~~~~~~~~~~~~");
		SendClientMessage(playerid, COLOR_WHITE, " Tropic: 454 || Tahoma: 566 || Tampa: 499 || Taxi: 420");
		SendClientMessage(playerid, COLOR_WHITE, " Topfun: 459 || Tornado: 576 || Towtruck: 525");
		SendClientMessage(playerid, COLOR_WHITE, " Trash: 408 || Tug: 583 || Turismo: 451 || Tram: 449 ");
		SendClientMessage(playerid, COLOR_WHITE, " Tugstair: 608 ");
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos T ~~~~~~~~~~~~~~~~~~~~~~~~");
		return 1;
	}

	if(strcmp(cmd, "/veiculosu", true) == 0)
	{
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos U ~~~~~~~~~~~~~~~~~~~~~~~~");
		SendClientMessage(playerid, COLOR_WHITE, " Uranus: 558 || Utility: 522 || Utiltr1: 611");
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos U ~~~~~~~~~~~~~~~~~~~~~~~~");
		return 1;
	}

	if(strcmp(cmd, "/veiculosv", true) == 0)
	{
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos V ~~~~~~~~~~~~~~~~~~~~~~~~");
		SendClientMessage(playerid, COLOR_WHITE, " Vortex: 539 || Vincent: 540 || Virgo: 491");
		SendClientMessage(playerid, COLOR_WHITE, " Vcnmav: 488 || Voodoo: 412");
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos V ~~~~~~~~~~~~~~~~~~~~~~~~");
		return 1;
	}

	if(strcmp(cmd, "/veiculosx", true) == 0)
	{
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos X ~~~~~~~~~~~~~~~~~~~~~~~~");
		SendClientMessage(playerid, COLOR_WHITE, " Não existe veículos com as inicias da letra 'X'");
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos X ~~~~~~~~~~~~~~~~~~~~~~~~");
		return 1;
	}

	if(strcmp(cmd, "/veiculosz", true) == 0)
	{
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos Z ~~~~~~~~~~~~~~~~~~~~~~~~");
		SendClientMessage(playerid, COLOR_WHITE, " Zr350: 477");
		SendClientMessage(playerid, verdel, "~~~~~~~~~~~~~~~~~~~~~~~~ Veículos Z ~~~~~~~~~~~~~~~~~~~~~~~~");
		return 1;
	}

	if(strcmp(cmd, "/conce", true) == 0)
	{
		if(IsPlayerInAnyVehicle(playerid))
		{
			SetPlayerInterior(playerid, 0);
			SetVehiclePosEx(GetPlayerVehicleID(playerid), 1002.3413,1710.9540,10.9219, 89.6340);
			LinkVehicleToInterior(GetPlayerVehicleID(playerid), 0);
		}
		else
		{
			SetPlayerInterior(playerid, 0);
			SetPlayerPos(playerid, 1002.3413,1710.9540,10.9219);
		}
		format(string, sizeof(string), "O(A) jogador(a) %s foi para a Concessionária (») Quer ir? Use: /conce", GetPlayerNameEx(playerid));
		SendClientMessageToAll(0x33AA33A, string);
		return 1;
	}

	if(strcmp(cmd, "/ajuda", true) == 0)
	{
		SendClientMessage(playerid, verdel, "====================== SERVER AJUDA =====================");
		SendClientMessage(playerid, COLOR_WHITE, "» Aqui você terá sua 2ª vida online, viva ela.");
		SendClientMessage(playerid, COLOR_WHITE, "» Adquira uma carteira de trabalho na delegacia.");
		SendClientMessage(playerid, COLOR_WHITE, "» Pegue um emprego na prefeitura de sua cidade.");
		SendClientMessage(playerid, COLOR_WHITE, "» Compre roupas em uma loja ou de algum(a) vendedor(a).");
		SendClientMessage(playerid, COLOR_WHITE, "» Abra uma conta no banco para gerenciar seu dinheiro.");
		SendClientMessage(playerid, verdel, "====================== SERVER AJUDA =====================");
		return 1;
	}

	if(strcmp(cmd, "/regras", true) == 0)
	{
		new strcmd[1000];

		strcat(strcmd, "{33AAFF}1ª Evite insultar, ofender, usar de meios para prejudicar outros jogadores.\n", sizeof(strcmd));
		strcat(strcmd, "{33AA33}2ª Não tente usar Xiters ou qualquer Mode para ter beneficios.\n", sizeof(strcmd));
		strcat(strcmd, "{33AAFF}3ª Não abuse de tais comandos de profissões existentes.\n", sizeof(strcmd));
		strcat(strcmd, "{33AA33}4ª O descomprimento de regras resultará em Banimento.\n", sizeof(strcmd));
		strcat(strcmd, "{33AAFF}5ª Seja honesto e jogue limpo conosco.", sizeof(strcmd));

		ShowPlayerDialog(playerid, Regras, DIALOG_STYLE_MSGBOX, ".:: - Regras do Servidor - ::.", strcmd, "Aceitar", "Recusar");
		return 1;
	}

	if(strcmp(cmd, "/tutorial", true) == 0)
	{
		DynTutorialStart(playerid);
		return 1;
	}

	if(strcmp(cmd, "/ajudalevel", true) == 0)
	{
		SendClientMessage(playerid, 0xFF8000AA, "======================== Ajuda Level ========================");
		SendClientMessage(playerid, COLOR_WHITE, "» Para você ganhar level(s) precisa juntar "#MAX_PLAYER_EXP" EXP's.");
		SendClientMessage(playerid, COLOR_WHITE, "» A cada "#TEMPO_EXP" minutos jogando você ganhará "#EXP_POR_TEMPO" EXP's.");
		SendClientMessage(playerid, COLOR_WHITE, "» Juntando "#MAX_PLAYER_EXP" EXP's você ganhará "#LEVEL_POR_EXP" level(s).");
		SendClientMessage(playerid, COLOR_WHITE, "» Você precisa ir estudar para ganhar "#ESTUDO_POR_TEMPO" de estudo por aula.");
		SendClientMessage(playerid, COLOR_WHITE, "» Veja seu level em /verlevel");
		SendClientMessage(playerid, 0xFF8000AA, "======================== Ajuda Level ========================");
		return 1;
	}

	if(strcmp(cmd, "/data", true) == 0)
	{
		new year, month, day,
			hour, minute, second;

		getdate(year, month, day);
		gettime(hour, minute, second);

		format(string, sizeof(string), "%d/%d/%d~n~%d:%d:%d", day, month, year, hour, minute, second);
		GameTextForPlayer(playerid, string, 5000, 1);
		return 1;
	}

/*
		DC's Server
			By Stremmer_Scripter#0961

	
*/

	if(strcmp("/comandos", cmd, true) == 0)
	{
		new strcmd[1000];

		strcat(strcmd, "{00FF00}» /animes /dance /teleportes /data /ausentes\n", sizeof(strcmd));
		strcat(strcmd, "{00FF00}» /regras /creditos /ajudalevel /vips /afk /on\n", sizeof(strcmd));
		strcat(strcmd, "{00FF00}» /procurados /profissao /saldocell /horarios\n", sizeof(strcmd));
		strcat(strcmd, "{00FF00}» /transferir /trocarsenha /trocarnick /mudarsexo\n", sizeof(strcmd));
		strcat(strcmd, "{00FF00}» /minhaprop /tutorial /ajudaprop /ajudacasa\n", sizeof(strcmd));
		strcat(strcmd, "{00FF00}» /minhacasa /meucarro /conce /p /verlevel\n", sizeof(strcmd));
		strcat(strcmd, "{00FF00}» /pref /dp /postols /postosf \n", sizeof(strcmd));
		strcat(strcmd, "{00FF00}» (Afim de ser um ADM?Use) /compraradm)\n", sizeof(strcmd));
		strcat(strcmd, "{00FF00}» (Afim de ser um Vip?Use) /comprarvip)\n", sizeof(strcmd));

		ShowPlayerDialog(playerid, playersimp, DIALOG_STYLE_MSGBOX, ".:: - Comandos Úteis - ::.", strcmd, "OK", "");
		return 1;
	}

	if(strcmp(cmd, "/profissao", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Desempregado)
		{
			SendClientMessage(playerid, Verde, "Desempregado");
		}
		if(dini_Int(file, "Profissao") == MotoristaP)
		{
			SendClientMessage(playerid, C_MotoristaP, "~~~~~~~~~~~~~~ Motorista Particular ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, 0xFFFFFFAA, "Arrume um patrão e cobre pelo seu serviço.");
			SendClientMessage(playerid, 0xFFFFFFAA, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_MotoristaP, "~~~~~~~~~~~~~~ Motorista Particular ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Guarda)
		{
			SendClientMessage(playerid, C_Guarda, "~~~~~~~~~~~~~~ Guarda ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/pegararmas [id]");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_Guarda, "~~~~~~~~~~~~~~ Guarda ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Policial_R)
		{
			SendClientMessage(playerid, C_PR, "~~~~~~~~~~~~~~ Policial Rodoviário ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/pegararmas [id]");
			SendClientMessage(playerid, Branco, "/multar [id] [quantia]");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_PR, "~~~~~~~~~~~~~~ Policial Rodoviário ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Policial_M)
		{
			SendClientMessage(playerid, C_PM, "~~~~~~~~~~~~~~ Policial Militar ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/pegararmas [id]");
			SendClientMessage(playerid, Branco, "/algemar [id]");
			SendClientMessage(playerid, Branco, "/desalgemar [id]");
			SendClientMessage(playerid, Branco, "/prender [id]");
			SendClientMessage(playerid, Branco, "/ad - /fd");
			SendClientMessage(playerid, Branco, "/ad2 - /fd2");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, Branco, "/paradoai [id]");
			SendClientMessage(playerid, C_PM, "~~~~~~~~~~~~~~ Policial Militar ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Policial_C)
		{
			SendClientMessage(playerid, C_PC, "~~~~~~~~~~~~~~ Policial Civil ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/pegararmas [id]");
			SendClientMessage(playerid, Branco, "/algemar [id]");
			SendClientMessage(playerid, Branco, "/desalgemar [id]");
			SendClientMessage(playerid, Branco, "/prender [id]");
			SendClientMessage(playerid, Branco, "/multar [id] [quantia]");
			SendClientMessage(playerid, Branco, "/ad - /fd");
			SendClientMessage(playerid, Branco, "/ad2 - /fd2");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, Branco, "/paradoai [id]");
			SendClientMessage(playerid, C_PC, "~~~~~~~~~~~~~~ Policial Civil ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Policial_F)
		{
			SendClientMessage(playerid, C_PF, "~~~~~~~~~~~~~~ Policial Federal ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/pegararmas [id]");
			SendClientMessage(playerid, Branco, "/algemar [id]");
			SendClientMessage(playerid, Branco, "/desalgemar [id]");
			SendClientMessage(playerid, Branco, "/prender [id]");
			SendClientMessage(playerid, Branco, "/multar [id] [quantia]");
			SendClientMessage(playerid, Branco, "/ad - /fd");
			SendClientMessage(playerid, Branco, "/ad2 - /fd2");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, Branco, "/paradoai [id]");
			SendClientMessage(playerid, C_PF, "~~~~~~~~~~~~~~ Policial Federal ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Delegado)
		{
			SendClientMessage(playerid, C_Delegado, "~~~~~~~~~~~~~~ Delegado ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/pegararmas [id]");
			SendClientMessage(playerid, Branco, "/algemar [id]");
			SendClientMessage(playerid, Branco, "/desalgemar [id]");
			SendClientMessage(playerid, Branco, "/prender [id]");
			SendClientMessage(playerid, Branco, "/multar [id] [quantia]");
			SendClientMessage(playerid, Branco, "/ad - /fd");
			SendClientMessage(playerid, Branco, "/ad2 - /fd2");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, Branco, "/paradoai [id]");
			SendClientMessage(playerid, C_Delegado, "~~~~~~~~~~~~~~ Delegado ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Bope)
		{
			SendClientMessage(playerid, C_Bope, "~~~~~~~~~~~~~~ B.O.P.E ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/pegararmas [id]");
			SendClientMessage(playerid, Branco, "/algemar [id]");
			SendClientMessage(playerid, Branco, "/desalgemar [id]");
			SendClientMessage(playerid, Branco, "/prender [id]");
			SendClientMessage(playerid, Branco, "/multar [id] [quantia]");
			SendClientMessage(playerid, Branco, "/ad - /fd");
			SendClientMessage(playerid, Branco, "/ad2 - /fd2");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, Branco, "/paradoai [id]");
			SendClientMessage(playerid, C_Bope, "~~~~~~~~~~~~~~ B.O.P.E ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Swat)
		{
			SendClientMessage(playerid, C_Swat, "~~~~~~~~~~~~~~ SWAT ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/pegararmas [id]");
			SendClientMessage(playerid, Branco, "/algemar [id]");
			SendClientMessage(playerid, Branco, "/desalgemar [id]");
			SendClientMessage(playerid, Branco, "/prender [id]");
			SendClientMessage(playerid, Branco, "/multar [id] [quantia]");
			SendClientMessage(playerid, Branco, "/ad - /fd");
			SendClientMessage(playerid, Branco, "/ad2 - /fd2");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, Branco, "/paradoai [id]");
			SendClientMessage(playerid, C_Swat, "~~~~~~~~~~~~~~ SWAT ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Narcoticos)
		{
			SendClientMessage(playerid, C_Narcoticos, "~~~~~~~~~~~~~~ Narcóticos ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/pegararmas [id]");
			SendClientMessage(playerid, Branco, "/algemar [id]");
			SendClientMessage(playerid, Branco, "/desalgemar [id]");
			SendClientMessage(playerid, Branco, "/prender [id]");
			SendClientMessage(playerid, Branco, "/multar [id] [quantia]");
			SendClientMessage(playerid, Branco, "/ad - /fd");
			SendClientMessage(playerid, Branco, "/ad2 - /fd2");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, Branco, "/paradoai [id]");
			SendClientMessage(playerid, C_Narcoticos, "~~~~~~~~~~~~~~ Narcóticos ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == TraficanteD)
		{
			SendClientMessage(playerid, C_TraficanteD, "~~~~~~~~~~~~~~ Traficante de Armas ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/venderak [id]");
			SendClientMessage(playerid, Branco, "/venderm4 [id]");
			SendClientMessage(playerid, Branco, "/venderswanoff [id]");
			SendClientMessage(playerid, Branco, "/vendersniper [id]");
			SendClientMessage(playerid, Branco, "/vendertec [id]");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_TraficanteD, "~~~~~~~~~~~~~~ Traficante de Armas ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Assasino)
		{
			SendClientMessage(playerid, C_Assasino, "~~~~~~~~~~~~~~ Assasino ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "Cobre por cada assasinato.");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_Assasino, "~~~~~~~~~~~~~~ Assasino ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Terrorista)
		{
			SendClientMessage(playerid, C_Terrorista, "~~~~~~~~~~~~~~ Terrorista ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/terror [texto]");
			SendClientMessage(playerid, Branco, "/plantarbomba");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_Terrorista, "~~~~~~~~~~~~~~ Terrorista ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Sequestrador)
		{
			SendClientMessage(playerid, C_Sequestrador, "~~~~~~~~~~~~~~ Sequestrador ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/avisar [texto]");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, Branco, "/sequestrar - /liberar");
			SendClientMessage(playerid, C_Sequestrador, "~~~~~~~~~~~~~~ Sequestrador ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == AssasinoProfissional)
		{
			SendClientMessage(playerid, C_AssasinoP, "~~~~~~~~~~~~~~ Assasino Profissional ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "Cobre por cada assasinato.");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_AssasinoP, "~~~~~~~~~~~~~~ Assasino Profissional ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Jornalista)
		{
			SendClientMessage(playerid, C_Jornalista, "~~~~~~~~~~~~~~ Jornalista ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/noticiar [texto]");
			SendClientMessage(playerid, Branco, "/convidarent [id]");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_Jornalista, "~~~~~~~~~~~~~~ Jornalista ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Fotografo)
		{
			SendClientMessage(playerid, C_Fotografo, "~~~~~~~~~~~~~~ Fotografo ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "Tire fotos para o jornal San Andreas.");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_Fotografo, "~~~~~~~~~~~~~~ Fotografo ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Reporter)
		{
			SendClientMessage(playerid, C_Reporter, "~~~~~~~~~~~~~~ Reporter ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/noticiar [texto]");
			SendClientMessage(playerid, Branco, "/convidarent [id]");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_Reporter, "~~~~~~~~~~~~~~ Reporter ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Ancora)
		{
			SendClientMessage(playerid, C_Ancora, "~~~~~~~~~~~~~~ Ancora ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/noticiar [texto]");
			SendClientMessage(playerid, Branco, "/convidarent [id]");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_Ancora, "~~~~~~~~~~~~~~ Ancora ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Meteorologista)
		{
			SendClientMessage(playerid, C_Meteoro, "~~~~~~~~~~~~~~ Meteorologista ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/previsao [previsão]");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_Meteoro, "~~~~~~~~~~~~~~ Meteorologista ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Mecanico)
		{
			SendClientMessage(playerid, C_Mecanico, "~~~~~~~~~~~~~~ Oficina ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/pintar [cor1] [cor2]");
			SendClientMessage(playerid, Branco, "/vcontrole");
			SendClientMessage(playerid, Branco, "/reparar");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_Mecanico, "~~~~~~~~~~~~~~ Oficina ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Rapper)
		{
			SendClientMessage(playerid, C_Rapper, "~~~~~~~~~~~~~~ Rapper ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/rimar [texto]");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_Rapper, "~~~~~~~~~~~~~~ Rapper ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == VendedorSkin)
		{
			SendClientMessage(playerid, C_VendedorSkin, "~~~~~~~~~~~~~~ Vendedor(a) de Skins ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/venderskin [id] [skin]");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_VendedorSkin, "~~~~~~~~~~~~~~ Vendedor(a) de Skins ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == VendedorCarro)
		{
			SendClientMessage(playerid, C_VendedorCarro, "~~~~~~~~~~~~~~ Vendedor(a) de Carros ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/setcar [id] [cor1] [cor2]");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_VendedorCarro, "~~~~~~~~~~~~~~ Vendedor(a) de Carros ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Frentista)
		{
			SendClientMessage(playerid, C_Frentista, "~~~~~~~~~~~~~~ Frentista ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/darcomb [id] [litros]");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_Frentista, "~~~~~~~~~~~~~~ Frentista ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Taxista)
		{
			SendClientMessage(playerid, C_Taxista, "~~~~~~~~~~~~~~ Taxista ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/ttaxi [id]");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_Taxista, "~~~~~~~~~~~~~~ Taxista ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Paramedico)
		{
			SendClientMessage(playerid, C_Paramedico, "~~~~~~~~~~~~~~ Paramédico ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/curativo [id]");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_Paramedico, "~~~~~~~~~~~~~~ Paramédico ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == FBI)
		{
			SendClientMessage(playerid, C_FBI, "~~~~~~~~~~~~~~ FBI ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/pegararmas [id]");
			SendClientMessage(playerid, Branco, "/algemar [id]");
			SendClientMessage(playerid, Branco, "/desalgemar [id]");
			SendClientMessage(playerid, Branco, "/prender [id]");
			SendClientMessage(playerid, Branco, "/multar [id] [quantia]");
			SendClientMessage(playerid, Branco, "/ad - /fd");
			SendClientMessage(playerid, Branco, "/ad2 - /fd2");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, Branco, "/paradoai [id]");
			SendClientMessage(playerid, C_FBI, "~~~~~~~~~~~~~~ FBI ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Interpol)
		{
			SendClientMessage(playerid, C_Interpol, "~~~~~~~~~~~~~~ Interpol ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/pegararmas [id]");
			SendClientMessage(playerid, Branco, "/algemar [id]");
			SendClientMessage(playerid, Branco, "/desalgemar [id]");
			SendClientMessage(playerid, Branco, "/prender [id]");
			SendClientMessage(playerid, Branco, "/multar [id] [quantia]");
			SendClientMessage(playerid, Branco, "/ad - /fd");
			SendClientMessage(playerid, Branco, "/ad2 - /fd2");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, Branco, "/paradoai [id]");
			SendClientMessage(playerid, C_Interpol, "~~~~~~~~~~~~~~ Interpol ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Aposentado)
		{
			SendClientMessage(playerid, C_Aposentado, "~~~~~~~~~~~~~~ Aposentado ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "Você está cansado da vida e não trabalha só recebe.");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_Aposentado, "~~~~~~~~~~~~~~ Aposentado ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Advogado)
		{
			SendClientMessage(playerid, C_Advogado, "~~~~~~~~~~~~~~ Advogado ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/soltar [id]");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_Advogado, "~~~~~~~~~~~~~~ Advogado ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == GuardaCostas)
		{
			SendClientMessage(playerid, C_GuardaCostas, "~~~~~~~~~~~~~~ Guarda Costas ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "Encontre um patrão e cobre pelo seu serviço.");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_GuardaCostas, "~~~~~~~~~~~~~~ Guarda Costas ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Corredor)
		{
			SendClientMessage(playerid, C_Corredor, "~~~~~~~~~~~~~~ Corredor de Rua ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/corrida [texto]");
			SendClientMessage(playerid, Branco, "Anuncie alguma corrida.");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_Corredor, "~~~~~~~~~~~~~~ Corredor de Rua ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Prostituta)
		{
			SendClientMessage(playerid, C_Prostituta, "~~~~~~~~~~~~~~ Prostituta ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "Cobre por cada encontro.");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_Prostituta, "~~~~~~~~~~~~~~ Prostituta ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Promoter)
		{
			SendClientMessage(playerid, C_Promoter, "~~~~~~~~~~~~~~ Promoter ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/festa [texto] - Anuncie uma festa.");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_Promoter, "~~~~~~~~~~~~~~ Promoter ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == SegVila)
		{
			SendClientMessage(playerid, C_SegVila, "~~~~~~~~~~~~~~ Segurança da Vila ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_SegVila, "~~~~~~~~~~~~~~ Segurança da Vila ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Assaltante)
		{
			SendClientMessage(playerid, C_SegVila, "~~~~~~~~~~~~~~ Assaltante ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "Seu trabalho e assaltar bancos!");
			SendClientMessage(playerid, Branco, "/amarrar [id]");
			SendClientMessage(playerid, Branco, "/desamarrar [id]");
			SendClientMessage(playerid, C_SegVila, "~~~~~~~~~~~~~~ Assaltante ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Bibliotecario)
		{
			SendClientMessage(playerid, C_Bibliotecario, "~~~~~~~~~~~~~~ Bibliotecário(a) ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "Você pode cuidar da biblioteca!");
			SendClientMessage(playerid, Branco, "/abrirbi");
			SendClientMessage(playerid, Branco, "/fecharbi");
			SendClientMessage(playerid, C_Bibliotecario, "~~~~~~~~~~~~~~ Bibliotecário(a) ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Traficante)
		{
			SendClientMessage(playerid, C_Traficante, "~~~~~~~~~~~~~~ Traficante ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/ofmaconha [id]");
			SendClientMessage(playerid, Branco, "/ofcrack [id]");
			SendClientMessage(playerid, Branco, "/ofcocaina [id]");
			SendClientMessage(playerid, C_Traficante, "~~~~~~~~~~~~~~ Traficante ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Prefeito)
		{
			SendClientMessage(playerid, C_Prefeito, "~~~~~~~~~~~~~~ Prefeito ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/discurso");
			SendClientMessage(playerid, Branco, "/soltar [id]");
			SendClientMessage(playerid, C_Prefeito, "~~~~~~~~~~~~~~ Prefeito ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Presidente)
		{
			SendClientMessage(playerid, C_Presidente, "~~~~~~~~~~~~~~ Presidente ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/discursar");
			SendClientMessage(playerid, Branco, "/soltar [id]");
			SendClientMessage(playerid, C_Presidente, "~~~~~~~~~~~~~~ Presidente ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Religioso)
		{
			SendClientMessage(playerid, C_Religioso, "~~~~~~~~~~~~~~ Pádre ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/abencoar [id]");
			SendClientMessage(playerid, C_Religioso, "~~~~~~~~~~~~~~ Pádre ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Mendigo)
		{
			SendClientMessage(playerid, C_Mendigo, "~~~~~~~~~~~~~~ Mendigo ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, 0xFFFFFFAA, "/esmola - Peça uma esmola.");
			SendClientMessage(playerid, 0xFFFFFFAA, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_Mendigo, "~~~~~~~~~~~~~~ Mendigo ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == LSPD)
		{
			SendClientMessage(playerid, C_LSPD, "~~~~~~~~~~~~~~ LSPD ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/pegararmas [id]");
			SendClientMessage(playerid, Branco, "/algemar [id]");
			SendClientMessage(playerid, Branco, "/desalgemar [id]");
			SendClientMessage(playerid, Branco, "/prender [id]");
			SendClientMessage(playerid, Branco, "/multar [id] [quantia]");
			SendClientMessage(playerid, Branco, "/ad - /fd");
			SendClientMessage(playerid, Branco, "/ad2 - /fd2");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, Branco, "/paradoai [id]");
			SendClientMessage(playerid, C_LSPD, "~~~~~~~~~~~~~~ LSPD ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Pedreiro)
		{
			SendClientMessage(playerid, C_Pedreiro, "~~~~~~~~~~~~~~ Pedreiro ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/reformar - Para reformar alguma coisa.");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_Pedreiro, "~~~~~~~~~~~~~~ Pedreiro ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Gari)
		{
			SendClientMessage(playerid, C_Gari, "~~~~~~~~~~~~~~ Gari ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/catarlixo - Para limpar a rua.");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_Gari, "~~~~~~~~~~~~~~ Gari ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Lixeiro)
		{
			SendClientMessage(playerid, C_Lixeiro, "~~~~~~~~~~~~~~ Lixeiro ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/catarlixo - Para pegar o lixo da rua.");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_Lixeiro, "~~~~~~~~~~~~~~ Lixeiro ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Temac)
		{
			SendClientMessage(playerid, C_Temac, "~~~~~~~~~~~~~~ Temac ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/trazer - Para dar assistencia a alguma pessoa.");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_Temac, "~~~~~~~~~~~~~~ Temac ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Correio)
		{
			SendClientMessage(playerid, C_Correio, "~~~~~~~~~~~~~~ Correios ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/publicar - Para publicar alguma coisa.");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_Correio, "~~~~~~~~~~~~~~ Correios ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Estudante)
		{
			SendClientMessage(playerid, C_Estudante, "~~~~~~~~~~~~~~ Estudante ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/horarios - Para ver os horários de funcionamento da biblioteca.");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_Estudante, "~~~~~~~~~~~~~~ Estudante ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Flanelinha)
		{
			SendClientMessage(playerid, C_Flanelinha, "~~~~~~~~~~~~~~ Flanelinha ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/vigiar - Para vigiar o carro de alguem.");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_Flanelinha, "~~~~~~~~~~~~~~ Flanelinha ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Cantor)
		{
			SendClientMessage(playerid, C_Cantor, "~~~~~~~~~~~~~~ Cantor(a) ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/cantar - Para cantar à todos do server.");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_Cantor, "~~~~~~~~~~~~~~ Cantor(a) ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Poeta)
		{
			SendClientMessage(playerid, C_Poeta, "~~~~~~~~~~~~~~ Poeta ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/poema - Para publicar um de seus poemas.");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_Poeta, "~~~~~~~~~~~~~~ Poeta ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Empregada)
		{
			SendClientMessage(playerid, C_Empregada, "~~~~~~~~~~~~~~ Empregada Doméstica ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/limparcasa - Encontre algum(a) patrão(oa) para te contratar.");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_Empregada, "~~~~~~~~~~~~~~ Empregada Doméstica ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == YKZ)
		{
			SendClientMessage(playerid, C_YKZ, "~~~~~~~~~~~~~~ Yakuza ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/venderak [id]");
			SendClientMessage(playerid, Branco, "/venderm4 [id]");
			SendClientMessage(playerid, Branco, "/venderswanoff [id]");
			SendClientMessage(playerid, Branco, "/vendersniper [id]");
			SendClientMessage(playerid, Branco, "/vendertec [id]");
			SendClientMessage(playerid, Branco, "/plantarbomba");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_YKZ, "~~~~~~~~~~~~~~ Yakuza ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == MRN)
		{
			SendClientMessage(playerid, C_MRN, "~~~~~~~~~~~~~~ Marines ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "Obedeça as ordens do General MacTavish.");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_MRN, "~~~~~~~~~~~~~~ Marines ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Mafia)
		{
			SendClientMessage(playerid, C_Mafia, "~~~~~~~~~~~~~~ Máfia ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/venderak [id]");
			SendClientMessage(playerid, Branco, "/venderm4 [id]");
			SendClientMessage(playerid, Branco, "/venderswanoff [id]");
			SendClientMessage(playerid, Branco, "/vendersniper [id]");
			SendClientMessage(playerid, Branco, "/vendertec [id]");
			SendClientMessage(playerid, Branco, "/sequestrar - /liberar");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_Mafia, "~~~~~~~~~~~~~~ Máfia ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Drifter)
		{
			SendClientMessage(playerid, C_Drifter, "~~~~~~~~~~~~~~ Drifter King ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "Melhore seu drift ao extremo.");
			SendClientMessage(playerid, Branco, "/dk - Anuncie um drift aqui.");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_Drifter, "~~~~~~~~~~~~~~ Drifter King ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Professor)
		{
			SendClientMessage(playerid, C_Professor, "~~~~~~~~~~~~~~ Professor ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/darestudo [id] [estudo]");
			SendClientMessage(playerid, Branco, "/abrirbi - /fecharbi");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_Professor, "~~~~~~~~~~~~~~ Professor ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Empregador)
		{
			SendClientMessage(playerid, C_Empregador, "~~~~~~~~~~~~~~ Empregador ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "/convidarent [id]");
			SendClientMessage(playerid, Branco, "/setprof [id] [prof]");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_Empregador, "~~~~~~~~~~~~~~ Empregador ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == AtiradorElite)
		{
			SendClientMessage(playerid, C_AtiradorElite, "~~~~~~~~~~~~~~ Atirador Elite ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "Os comandos se encontram em construção.");
			SendClientMessage(playerid, Branco, " ");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_AtiradorElite, "~~~~~~~~~~~~~~ Atirador Elite ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Ninja)
		{
			SendClientMessage(playerid, C_Ninja, "~~~~~~~~~~~~~~ Ninja ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "Especialista em mortes silênciadas.");
			SendClientMessage(playerid, Branco, "/na - Anúncie sua profissão.");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_Ninja, "~~~~~~~~~~~~~~ Ninja ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Maquinista)
		{
			SendClientMessage(playerid, C_Maquinista, "~~~~~~~~~~~~~~ Maquinista ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "Pegue o trem para fazer seus serviços.");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_Maquinista, "~~~~~~~~~~~~~~ Maquinista ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Caminhoneiro)
		{
			SendClientMessage(playerid, C_Caminhoneiro, "~~~~~~~~~~~~~~ Caminhoneiro ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "Pegue um caminhão para fazer seus serviços.");
			SendClientMessage(playerid, Branco, "/descarregar - Para deixar sua carga em algum polo.");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_Caminhoneiro, "~~~~~~~~~~~~~~ Caminhoneiro ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == Petroleiro)
		{
			SendClientMessage(playerid, C_Petroleiro, "~~~~~~~~~~~~~~ Petroleiro ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "Pegue um caminhão para fazer seus serviços.");
			SendClientMessage(playerid, Branco, "/descarregar - Para deixar sua carga em algum polo.");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_Petroleiro, "~~~~~~~~~~~~~~ Petroleiro ~~~~~~~~~~~~~~");
		}
		if(dini_Int(file, "Profissao") == MOD)
		{
			SendClientMessage(playerid, C_MOD, "~~~~~~~~~~~~~~ Moderador ~~~~~~~~~~~~~~");
			SendClientMessage(playerid, Branco, "Seu dever de moderador é ajudar os outros em tudo.");
			SendClientMessage(playerid, Branco, "/comandosadm - Para ver os comandos que poderá usar.");
			SendClientMessage(playerid, Branco, "/cp - Chat Profissão.");
			SendClientMessage(playerid, C_MOD, "~~~~~~~~~~~~~~ Moderador ~~~~~~~~~~~~~~");
		}
		return 1;
	}

	if(!strcmp(cmd, "/comemorar", true))
	{
		if(pAdmin[playerid] > 0 || IsPlayerVIP(playerid))
		{
			new Float:x, Float:y, Float:z;

			GetPlayerPos(playerid, x, y, z);

			SetTimerEx("CriarExplosao", 1000, false, "fffdf", x+1, y+1, z+12, 10, 100.0);
			SetTimerEx("CriarExplosao", 2000, false, "fffdf", x+5, y+5, z+12, 10, 100.0);
			SetTimerEx("CriarExplosao", 3000, false, "fffdf", x+9, y+9, z+12, 10, 100.0);
			SetTimerEx("CriarExplosao", 4000, false, "fffdf", x+1, y+1, z+12, 10, 100.0);
			SetTimerEx("CriarExplosao", 5000, false, "fffdf", x+5, y+5, z+12, 10, 100.0);
			SetTimerEx("CriarExplosao", 6000, false, "fffdf", x+9, y+9, z+12, 10, 100.0);
			SetTimerEx("CriarExplosao", 7000, false, "fffdf", x+1, y+1, z+12, 10, 100.0);
			SetTimerEx("CriarExplosao", 8000, false, "fffdf", x+5, y+5, z+12, 10, 100.0);
			SetTimerEx("CriarExplosao", 9000, false, "fffdf", x+9, y+9, z+12, 10, 100.0);

			GameTextForPlayer(playerid, "~r~FOGOS ~y~DE ~b~ARTIFICIO", 1000, 4);
		}
		return 1;
	}

	if(!strcmp(cmd, "/plantarbomba", true))
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Terrorista || dini_Int(file, "Profissao") == YKZ || dini_Int(file, "aAdmin") == 1 || PlayerInfo[playerid][SCON] == true)
		{
			if(pbomba[playerid] == 0)
			{
				if(IsPlayerInAnyVehicle(playerid))
				{
					SendClientMessage(playerid, Vermelho, "Você não pode plantar uma bomba dentro do veículo.");
				}
				else
				{
					new dinid, Float:x, Float:y, Float:z;

					pbomba[playerid] = 1;
					GetPlayerPos(playerid, x, y, z);

					#if defined AnimLoopsUser
					OnePlayAnim(playerid, "BOMBER", "BOM_Plant", 4.0, 0, 0, 0, 0, 0);
					#else
					ApplyAnimation(playerid, "BOMBER", "BOM_Plant", 4.0, 0, 0, 0, 0, 0);
					#endif

					dinid = CreateDynamicObject(1252, x, y, z-0.8, 0, 0, 0, -1, -1, -1, 200.0);
					SetTimerEx("DestruirObjeto", 8000, false, "d", dinid);
					SetTimerEx("PlantouBomba", 7000, false, "i", playerid);

					SetTimerEx("CriarExplosao", 8000, false, "fffdf", x, y, z-0.8, 7, 50.0);
					SetTimerEx("CriarExplosao", 8000, false, "fffdf", x, y, z-0.8, 10, 50.0);
					SetTimerEx("CriarExplosao", 8000, false, "fffdf", x, y, z-0.8, 2, 50.0);

					GameTextForPlayer(playerid, "~r~BOMBA ~b~PLANTADA", 1000, 4);
				}
			}
		}
		else
		{
			SendClientMessage(playerid, Verde, "Plante uma bomba por vez.");
		}
		return 1;
	}

	if(strcmp(cmd, "/taxi", true) == 0)
	{
		new texto[128];

		if(sscanf(cmdtext, "s[6]s[128]", cmd, texto))
		{
			SendClientMessage(playerid, Vermelho, "Use: /taxi [lugar]");
		}
		else
		{
			for(new i = 0; i < MAX_PLAYERS; i++)
			{
				if(IsPlayerConnected(i))
				{
					format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(i));
					if(dini_Int(file, "Profissao") == Taxista || dini_Int(file, "aAdmin") == 1)
					{
						format(string, sizeof(string), "%s pediu um Taxi (») Local: %s (») Atenda-o e não esqueça de ligar o taximetro.", GetPlayerNameEx(playerid), texto);
						SendClientMessage(i, Amarelo, string);
					}
				}
			}
			SendClientMessage(playerid, Amarelo, "Os taxistas foram informados, aguarde uma resposta.");
		}
		return 1;
	}

	if(strcmp(cmd, "/190", true) == 0)
	{
		new texto[128];

		if(sscanf(cmdtext, "s[5]s[128]", cmd, texto))
		{
			SendClientMessage(playerid, Vermelho, "Use: /190 [denúncia]");
		}
		else
		{
			for(new i = 0; i < MAX_PLAYERS; i++)
			{
				if(IsPlayerConnected(i))
				{
					format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(i));
					if(dini_Int(file, "Profissao") == Guarda || dini_Int(file, "Profissao") == Policial_R  || dini_Int(file, "Profissao") == Policial_M || dini_Int(file, "Profissao") == Policial_C || dini_Int(file, "Profissao") == Policial_F || dini_Int(file, "Profissao") == Delegado || dini_Int(file, "Profissao") == Bope || dini_Int(file, "Profissao") == Swat || dini_Int(file, "Profissao") == Narcoticos || dini_Int(file, "Profissao") == Interpol || dini_Int(file, "Profissao") == LSPD)
					{
						format(string, sizeof(string), "%s fez uma denúncia: %s (») Atenda-o!", GetPlayerNameEx(playerid), texto);
						SendClientMessage(i, Blue, string);
					}
				}
			}
			SendClientMessage(playerid, Blue, "Os policiais foram informados, aguarde uma resposta.");
		}
		return 1;
	}

	if(strcmp(cmd, "/193", true) == 0)
	{
		new texto[128];

		if(sscanf(cmdtext, "s[5]s[128]", cmd, texto))
		{
			SendClientMessage(playerid, Vermelho, "Use: /193 [local]");
		}
		else
		{
			for(new i = 0; i < MAX_PLAYERS; i++)
			{
				if(IsPlayerConnected(i))
				{
					format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(i));
					if(dini_Int(file, "Profissao") == Paramedico || dini_Int(file, "Profissao") == Guarda || dini_Int(file, "Profissao") == Policial_R  || dini_Int(file, "Profissao") == Policial_M || dini_Int(file, "Profissao") == Policial_C || dini_Int(file, "Profissao") == Policial_F || dini_Int(file, "Profissao") == Delegado || dini_Int(file, "Profissao") == Bope || dini_Int(file, "Profissao") == Swat || dini_Int(file, "Profissao") == Narcoticos || dini_Int(file, "aAdmin") == 1 || PlayerInfo[playerid][SCON] == true)
					{
						format(string, sizeof(string), "%s pedindu uma ambulância (») Local: %s (») Atenda-o rápido!", GetPlayerNameEx(playerid), texto);
						SendClientMessage(i, Blue, string);
					}
				}
			}
			SendClientMessage(playerid, Blue, "Os paramédicos foram informados, aguarde uma resposta.");
		}
		return 1;
	}

	if(strcmp(cmd, "/curativo", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Paramedico || dini_Int(file, "aAdmin") == 1 || PlayerInfo[playerid][SCON] == true)
		{
			new plid;

			if(sscanf(cmdtext, "s[10]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "Use: /curativo [id]");
				return 1;
			}
			if(IsPlayerInAnyVehicle(plid) || IsPlayerInAnyVehicle(playerid))
			{
				SendClientMessage(playerid, Vermelho, "Alguem está dentro de um carro.");
				return 1;
			}
			if(plid == playerid)
			{
				SendClientMessage(playerid, Vermelho, "Você não consegue curar você mesmo.");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				if(GetDistanceBetweenPlayers(plid, playerid) < 21)
				{
					SetPlayerHealth(plid, 100.0);
					SendClientMessage(playerid, COLOR_GREEN, "Missão cumprida!");
					SendClientMessage(plid, COLOR_WHITE, "Um paramédico aplicou uma injeção e você melhoro!");
				}
				else
				{
					SendClientMessage(playerid, Vermelho, "Chegue mais perto para aplicar o curativo!");
				}
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "O(A) jogador(a) não está conectado.");
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Apenas paramédicos podem usar este comando!");
		}
		return 1;
	}

	if(strcmp(cmd, "/adminsrcon", true) == 0)
	{
		new count = 0, msg[1000];

		for(new i = 0; i < MAX_PLAYERS; i++)
		{
			if(IsPlayerConnected(i) && PlayerInfo[i][SCON] == true)
			{
				format(msg,sizeof(msg), "{FFFFFF}» %s (ID: %d)\n", GetPlayerNameEx(i), i);
				strcat(string, msg, sizeof(string));
				count++;
			}
		}
		if(count == 0)
		{
			ShowPlayerDialog(playerid, playersimp, DIALOG_STYLE_MSGBOX, ".:: - RCON's Online - ::.", "{FF0000}Não há nenhum ADM RCON online no momento.", "OK", "");
		}
		else
		{
			ShowPlayerDialog(playerid, playersimp, DIALOG_STYLE_MSGBOX, ".:: - RCON's Online - ::.", string, "OK", "");
		}
		return 1;
	}

/*
		DC's Server
			By Stremmer_Scripter#0961


*/

	if(strcmp("/creditos", cmd, true) == 0)
	{
		new strcmd[1000], strurl[128];
		format(strurl, sizeof(strurl), "\t{FFFFFF}Site: {FF0000}%s\n", weburl);

		strcat(strcmd, "\t{FFFFFF}Criado por: {FF0000}[DC]Stremmer[DONO]\n\n", sizeof(strcmd));
		strcat(strcmd, "\t{FFFFFF}Editado por: {FF0000}[DC]Stremmer[DONO]\n\n", sizeof(strcmd));
		strcat(strcmd, "{FFFFFF}Game Mode: {FF0000} DUBAY CITY[RPG] v5.0\n\n", sizeof(strcmd));
		strcat(strcmd, strurl, sizeof(strcmd));
		strcat(strcmd, "\t{FFFFFF}Discord: Stremmer_Scripter#0961 ", sizeof(strcmd));
		strcat(strcmd, "\t{FFFFFF}Server Discord: https://discord.gg/k5JCCDxVAt", sizeof(strcmd));

		ShowPlayerDialog(playerid, Creditos, DIALOG_STYLE_MSGBOX, "Creditos", strcmd, "OK", "");
		return 1;
	}

	if(strcmp("/setarvip", cmd, true) == 0)
	{
		if(PlayerInfo[playerid][SCON] == true)
		{
			new plid, dias;

			if(sscanf(cmdtext, "s[10]ud", cmd, plid, dias))
			{
				SendClientMessage(playerid, 0x008040AA, "Use: /setarvip [id] [quantidade-de-dias]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				if(dias > 30 || dias < 1)
				{
					SendClientMessage(playerid, Vermelho, "Não pode setar mais de 30 dias ou menos de 1 dia!");
				}
				else
				{
					if(GetVIPDays(plid) > 5)
					{
						SendClientMessage(playerid, Vermelho, "Este jogador ainda tem mais de 5 dias VIP.");
					}
					else
					{
						SetPlayerVIP(plid, dias);
						format(string, sizeof(string), "%s (%d) (») Promoveu: %s (%d) para VIP (») Por %d dia(s)!", GetPlayerNameEx(playerid), playerid, GetPlayerNameEx(plid), plid, dias);
						SendClientMessageToAll(tcadm, string);
					}
				}
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente!");
			}
		}
		return 1;
	}

	if(strcmp("/tirarvip", cmd, true) == 0)
	{
		if(PlayerInfo[playerid][SCON] == true)
		{
			new plid;

			if(sscanf(cmdtext, "s[10]u", cmd, plid))
			{
				SendClientMessage(playerid, 0x008040AA, "Use: /tirarvip [id]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				UnsetPlayerVIP(plid);
				format(string, sizeof(string), "%s (%d) (») Retirou o VIP do(a): %s (%d)", GetPlayerNameEx(playerid), playerid, GetPlayerNameEx(plid), plid);
				SendClientMessageToAll(tcadm, string);
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente!");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/tirarvipdetodos", true) == 0)
	{
		if(PlayerInfo[playerid][SCON] == true)
		{
			for(new i = 0; i < MAX_PLAYERS; i++)
			{
				if(IsPlayerConnected(i))
				{
					if(IsPlayerVIP(i))
					{
						UnsetPlayerVIP(i);
					}
				}
			}
			SendClientMessage(playerid, Verde, "Concluído somente para os Online!");
		}
		return 1;
	}

	if(strcmp(cmd, "/eusouvip", true) == 0)
	{
		if(!IsPlayerVIP(playerid))
		{
			SendClientMessage(playerid, 0xFFFF00AA, "Você não é um player VIP!");
		}
		else
		{
			format(string, sizeof(string), "Eu %s sou 'VIP' então me chupa!", GetPlayerNameEx(playerid));
			SendClientMessageToAll(roxo, string);
		}
		return 1;
	}

	if(strcmp(cmd, "/vips", true) == 0)
	{
		new count = 0, msg[1000];

		for(new i = 0; i < MAX_PLAYERS; i++)
		{
			if(IsPlayerConnected(i) && IsPlayerVIP(i))
			{
				format(msg, sizeof(msg), "{FFFFFF}» %s (ID: %d)\n", GetPlayerNameEx(i), i);
				strcat(string, msg, sizeof(string));
				count++;
			}
		}
		if(count == 0)
		{
			ShowPlayerDialog(playerid, playersimp, DIALOG_STYLE_MSGBOX, ".:: - VIP's Online - ::.", "{FF0000}Nenhum VIP está online no momento.", "OK", "");
		}
		else
		{
			ShowPlayerDialog(playerid, playersimp, DIALOG_STYLE_MSGBOX, ".:: - VIP's Online - ::.", string, "OK", "");
		}
		return 1;
	}

	if(strcmp(cmd, "/npcs", true) == 0)
	{
		new count = 0, msg[1000];

		for(new i = 0; i < MAX_PLAYERS; i++)
		{
			if(IsPlayerConnected(i) && IsPlayerNPC(i))
			{
				format(msg, sizeof(msg), "{FFFFFF}» %s (ID: %d)\n", GetPlayerNameEx(i), i);
				strcat(string, msg, sizeof(string));
				count++;
			}
		}
		if(count == 0)
		{
			ShowPlayerDialog(playerid, playersimp, DIALOG_STYLE_MSGBOX, ".:: - NPC's Online - ::.", "{FF0000}Nenhum NPC está conectado no momento.", "OK", "");
		}
		else
		{
			ShowPlayerDialog(playerid, playersimp, DIALOG_STYLE_MSGBOX, ".:: - NPC's Online - ::.", string, "OK", "");
		}
		return 1;
	}

	if(strcmp(cmd, "/casasdeletadas", true) == 0)
	{
		new count = 0, msg[1000];

		SendClientMessage(playerid, 0x008000AA, "=== Casas Deletadas ===");
		for(new c = 0; c < MAX_CASAS; c++)
		{
			format(string, sizeof(string), PASTA_CASAS, c);
			if(dini_Int(string, "TDono") == 3)
			{
				format(msg, sizeof(msg), "» Casa ID: %d - Deletada", c);
				SendClientMessage(playerid, 0x0088CAAA, msg);
				count++;
			}
		}
		if(count == 0)
		{
			SendClientMessage(playerid, 0xFF0000AA, "Nenhuma casa foi deletada!");
		}
		return 1;
	}

	if(strcmp(cmd, "/propsdeletadas", true) == 0)
	{
		new count = 0, msg[1000];

		SendClientMessage(playerid, 0x008000AA, "=== Propriedades Deletadas ===");
		for(new p = 0; p < MAX_PROPS; p++)
		{
			format(string, sizeof(string), PASTA_PROPS, p);
			if(dini_Int(string, "TDono") == 3)
			{
				format(msg, sizeof(msg), "» Prop ID: %d - Deletada", p);
				SendClientMessage(playerid, 0x0088CAAA, msg);
				count++;
			}
		}
		if(count == 0)
		{
			SendClientMessage(playerid, 0xFF0000AA, "Nenhuma propriedade foi deletada!");
		}
		return 1;
	}

	if(strcmp(cmd, "/trocarnick", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "MN") == 1)
		{
			trocandonick[playerid] = 1;
			ShowPlayerDialog(playerid, mudarnick, DIALOG_STYLE_INPUT, "Trocando Nick", "{33AA33}Digite seu novo nick:", "Mudar", "Sair");
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Peça para um ADM te liberar a mudar de nick!");
		}
		return 1;
	}

	if(strcmp(cmd, "/trocarsenha", true) == 0)
	{
		ShowPlayerDialog(playerid, mudarsenha, DIALOG_STYLE_PASSWORD, "Trocando Senha", "{FF0000}Digite sua nova senha:", "Mudar", "Sair");
		return 1;
	}

	if(strcmp(cmd, "/mudarsexo", true) == 0)
	{
		ShowPlayerDialog(playerid, skinnovato, DIALOG_STYLE_MSGBOX, "Gênero", "{00ff00}Qual é seu sexo?\n\n", "Masculino", "Feminino");
		return 1;
	}

	if(strcmp(cmd, "/playstream", true) == 0)
	{
		#if defined AudioStreamUser
		ShowPlayerDialog(playerid, streamlink, DIALOG_STYLE_LIST, WHOMADETHIS, "{46BEE6}Reproduzir para Mim\n{ED954E}Reproduzir para Outro\n{46BEE6}Reproduzir na minha Localização\n{ED954E}Reproduzir para Todos", "OK", "Cancel");
		#else
		SendClientMessage(playerid, GREEN, "Comando desativado temporáriamente.");
		#endif

		return 1;
	}

	if(strcmp(cmd, "/stopstream", true) == 0)
	{
		#if defined AudioStreamUser
		StopAudioStreamForPlayer(playerid);
		SendClientMessage(playerid, GREEN, "Você parou o streaming de audio.");
		#else
		SendClientMessage(playerid, GREEN, "Comando desativado temporáriamente.");
		#endif

		return 1;
	}

	if(strcmp(cmd, "/radio", true) == 0)
	{
		#if defined AudioStreamUser
		PlayerInitDialog(playerid);
		#else
		SendClientMessage(playerid, GREEN, "Comando desativado temporáriamente.");
		#endif

		return 1;
	}

	if(strcmp(cmd, "/comandosmp3", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "MP3") == 1)
		{
			SendClientMessage(playerid, Verde, "Digite /playmp3 para começar e /stopmp3 para parar.");
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem um mp3 para usar seu manual.");
		}
		return 1;
	}

	if(strcmp(cmd, "/playmp3", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "MP3") == 1)
		{
			ShowPlayerDialog(playerid, mp3, DIALOG_STYLE_LIST, "Escolher Música", STRD_MP3, "OK", "Cancelar");
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem um mp3 para escutar, você pode comprar um em uma loja de ultilidades.");
		}
		return 1;
	}

	if(strcmp(cmd, "/stopmp3", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "MP3") == 1)
		{
			#if defined AudioStreamUser
			StopAudioStreamForPlayer(playerid);
			#else
			new Float:X, Float:Y, Float:Z;

			GetPlayerPos(playerid, X, Y, Z);
			PlayerPlaySound(playerid, 1063, X, Y, Z);
			PlayerPlaySound(playerid, 1069, X, Y, Z);
			PlayerPlaySound(playerid, 1077, X, Y, Z);
			PlayerPlaySound(playerid, 1098, X, Y, Z);
			PlayerPlaySound(playerid, 1184, X, Y, Z);
			PlayerPlaySound(playerid, 1186, X, Y, Z);
			PlayerPlaySound(playerid, 1188, X, Y, Z);
			#endif
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem um mp3 para dar stop.");
		}
		return 1;
	}

	if(strcmp(cmd, "/abastecer", true) == 0)
	{
		new quantidade, quantia;

		if(sscanf(cmdtext, "s[11]d", cmd, quantidade))
		{
			SendClientMessage(playerid, Vermelho, "Use: /abastecer [litros]");
			return 1;
		}
		if(quantidade <= 0 || GetPlayerGrana(playerid) < quantidade)
		{
			SendClientMessage(playerid, Vermelho, "Você não tem dinheiro suficiente para pagar o frentista.");
			return 1;
		}
		if(AreaPosto[playerid] == 1)
		{
			if(IsPlayerInAnyVehicle(playerid) == 1)
			{
				for(new carro = 0; carro < MAX_CONCES; carro++)
				{
					format(file, sizeof(file), PASTA_CONCE, carro);
					if(dini_Exists(file))
					{
						if(GetPlayerVehicleID(playerid) == dini_Int(file, "Id"))
						{
							if(quantidade+dini_Int(file, "Combustivel") < MAX_COMB)
							{
								GivePlayerGrana(playerid, -quantidade);
								dini_IntSet(file, "Combustivel", dini_Int(file, "Combustivel")+quantidade);

								new engine, lights, alarm, doors, bonnet, boot, objective;
								GetVehicleParamsEx(dini_Int(file, "Id"), engine, lights, alarm, doors, bonnet, boot, objective);
								SetVehicleParamsEx(dini_Int(file, "Id"), VEHICLE_PARAMS_ON, lights, alarm, doors, bonnet, boot, objective);

								format(string, sizeof(string), "Você completou seu tanque com %d litro(s) de combustível.", quantidade);
								SendClientMessage(playerid, COLOR_GREEN, string);
							}
							else if(quantidade+dini_Int(file, "Combustivel"))
							{
								GivePlayerGrana(playerid, -quantia);
								quantia = MAX_COMB-dini_Int(file, "Combustivel");
								dini_IntSet(file, "Combustivel", MAX_COMB);

								new engine, lights, alarm, doors, bonnet, boot, objective;
								GetVehicleParamsEx(dini_Int(file, "Id"), engine, lights, alarm, doors, bonnet, boot, objective);
								SetVehicleParamsEx(dini_Int(file, "Id"), VEHICLE_PARAMS_ON, lights, alarm, doors, bonnet, boot, objective);

								format(string, sizeof(string), "Tanque cheio, foram colocados %d litro(s).", quantia);
								SendClientMessage(playerid, COLOR_GREEN, string);
							}
							return 1;
						}
					}
				}
				SendClientMessage(playerid, COLOR_RED, "Você não está em um veículo da Conce.");
			}
			else
			{
				format(file2, sizeof(file2), PASTA_CONTAS, GetPlayerNameEx(playerid));
				if(quantidade+dini_Int(file2, "Combustivel") < MAX_COMB)
				{
					GivePlayerGrana(playerid, -quantidade);
					dini_IntSet(file2, "Combustivel", dini_Int(file2, "Combustivel")+quantidade);

					format(string, sizeof(string), "Você encheu galões com %d litro(s) de combustível.", quantidade);
					SendClientMessage(playerid, COLOR_GREEN, string);
				}
				else if(quantidade+dini_Int(file2, "Combustivel"))
				{
					GivePlayerGrana(playerid, -quantia);
					quantia = MAX_COMB-dini_Int(file2, "Combustivel");
					dini_IntSet(file2, "Combustivel", MAX_COMB);

					format(string, sizeof(string), "Galões cheio, foram colocados %d litro(s).", quantia);
					SendClientMessage(playerid, COLOR_GREEN, string);
				}
				ShowPlayerDialog(playerid, playersimp, DIALOG_STYLE_MSGBOX, "Posto de Combustível", "{FFFF00}Você não está em um veículo.\n{FFFFFF}Enchemos galões de combustível para você.\n{FF33FF}Vá até seu carro e use /enchertanque para colocar este combustível.", "OK", "");
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não está no posto.");
		}
		return 1;
	}

	if(strcmp(cmd, "/enchertanque", true) == 0)
	{
		new quantidade, quantia;

		if(IsPlayerInAnyVehicle(playerid) == 1)
		{
			format(file2, sizeof(file2), PASTA_CONTAS, GetPlayerNameEx(playerid));
			quantidade = dini_Int(file2, "Combustivel");
			if(quantidade > 0)
			{
				for(new carro = 0; carro < MAX_CONCES; carro++)
				{
					format(file, sizeof(file), PASTA_CONCE, carro);
					if(dini_Exists(file))
					{
						if(GetPlayerVehicleID(playerid) == dini_Int(file, "Id"))
						{
							if(quantidade+dini_Int(file, "Combustivel") < MAX_COMB)
							{
								dini_IntSet(file2, "Combustivel", dini_Int(file2, "Combustivel")-quantidade);
								dini_IntSet(file, "Combustivel", dini_Int(file, "Combustivel")+quantidade);

								new engine, lights, alarm, doors, bonnet, boot, objective;
								GetVehicleParamsEx(dini_Int(file, "Id"), engine, lights, alarm, doors, bonnet, boot, objective);
								SetVehicleParamsEx(dini_Int(file, "Id"), VEHICLE_PARAMS_ON, lights, alarm, doors, bonnet, boot, objective);

								format(string, sizeof(string), "Você completou seu tanque com %d litro(s) de combustível.", quantidade);
								SendClientMessage(playerid, COLOR_GREEN, string);

								format(string, sizeof(string), "Resta %d litro(s) em seus galões.", dini_Int(file2, "Combustivel"));
								SendClientMessage(playerid, Amarelo, string);
							}
							else if(quantidade+dini_Int(file, "Combustivel"))
							{
								quantia = MAX_COMB-dini_Int(file, "Combustivel");
								dini_IntSet(file2, "Combustivel", dini_Int(file2, "Combustivel")-quantia);
								dini_IntSet(file, "Combustivel", MAX_COMB);

								new engine, lights, alarm, doors, bonnet, boot, objective;
								GetVehicleParamsEx(dini_Int(file, "Id"), engine, lights, alarm, doors, bonnet, boot, objective);
								SetVehicleParamsEx(dini_Int(file, "Id"), VEHICLE_PARAMS_ON, lights, alarm, doors, bonnet, boot, objective);

								format(string, sizeof(string), "Tanque cheio, foram colocados %d litro(s).", quantia);
								SendClientMessage(playerid, COLOR_GREEN, string);

								format(string, sizeof(string), "Resta %d litro(s) em seus galões.", dini_Int(file2, "Combustivel"));
								SendClientMessage(playerid, Amarelo, string);
							}
							return 1;
						}
					}
				}
				SendClientMessage(playerid, COLOR_RED, "Você não está em um veículo da Conce.");
			}
			else
			{
				SendClientMessage(playerid, COLOR_RED, "Você não tem galões cheios.");
			}
		}
		else
		{
			SendClientMessage(playerid, COLOR_RED, "Entre em seu carro para abastece-lo.");
		}
		return 1;
	}

	if(strcmp(cmd, "/pegararmas", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Policial_M || dini_Int(file, "Profissao") == Policial_C || dini_Int(file, "Profissao") == Policial_F || dini_Int(file, "Profissao") == Delegado || dini_Int(file, "Profissao") == Bope || dini_Int(file, "Profissao") == Swat || dini_Int(file, "Profissao") == Narcoticos || dini_Int(file, "Profissao") == FBI || dini_Int(file, "Profissao") == Interpol || dini_Int(file, "Profissao") == LSPD || dini_Int(file, "aAdmin") == 1)
		{
			new plid;

			if(sscanf(cmdtext, "s[12]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "Use: /pegararmas [id]");
				return 1;
			}
			if(IsPlayerInAnyVehicle(plid) || IsPlayerInAnyVehicle(playerid))
			{
				SendClientMessage(playerid, Vermelho, "Alguem está dentro de um carro.");
				return 1 ;
			}
			if(plid == playerid)
			{
				SendClientMessage(playerid, Vermelho, "Você não pode fazer isto com você mesmo.");
				return 1 ;
			}
			if(!IsPlayerConnected(plid))
			{
				SendClientMessage(playerid, Vermelho, "O(A) jogador(a) não está conectado.");
			}
			else
			{
				if(GetDistanceBetweenPlayers(plid, playerid) < 10)
				{
					ResetPlayerWeapons(plid);

					format(string, sizeof(string), "O(A) policial %s prendeu suas armas, elas foram levadas para delegacia.", GetPlayerNameEx(playerid));
					SendClientMessage(plid, Vermelho, string);

					SendClientMessage(playerid, COLOR_GREEN, "Armas apreendidas com sucesso.");
				}
				else
				{
					SendClientMessage(playerid, Vermelho, "Chegue mais perto para apreender as armas.");
				}
			}
		}
		else
		{
			SendClientMessage(playerid,Vermelho, "Apenas policiais podem apreender armas.");
		}
		return 1;
	}

	if(strcmp(cmd, "/paradoai", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Policial_M || dini_Int(file, "Profissao") == Policial_C || dini_Int(file, "Profissao") == Policial_F || dini_Int(file, "Profissao") == Delegado || dini_Int(file, "Profissao") == Bope || dini_Int(file, "Profissao") == Swat || dini_Int(file, "Profissao") == Narcoticos || dini_Int(file, "Profissao") == FBI || dini_Int(file, "Profissao") == Interpol || dini_Int(file, "Profissao") == LSPD || dini_Int(file, "aAdmin") == 1)
		{
			new plid;

			if(sscanf(cmdtext, "s[10]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "Use: /paradoai [id]");
				return 1;
			}
			if(IsPlayerInAnyVehicle(plid) || IsPlayerInAnyVehicle(playerid))
			{
				SendClientMessage(playerid, Vermelho, "Alguem está dentro de um carro.");
				return 1 ;
			}
			if(plid == playerid)
			{
				SendClientMessage(playerid, Vermelho, "Você não pode ultilizar este comandos com você mesmo.");
				return 1 ;
			}
			if(!IsPlayerConnected(plid))
			{
				SendClientMessage(playerid, Vermelho, "O(A) jogador(a) não está conectado.");
			}
			else
			{
				if(Procurados[plid] == 1 || infratores[plid] == 1)
				{
					if(GetDistanceBetweenPlayers(plid, playerid) < 10)
					{
						SetPlayerSpecialAction(plid, SPECIAL_ACTION_HANDSUP);

						format(string, sizeof(string), "O(A) policial %s te deu voz de prisão, portanto fique parado e escute ele.", GetPlayerNameEx(playerid));
						SendClientMessage(plid, Blue, string);

						SendClientMessage(playerid, COLOR_GREEN, "Comando efetuado com sucesso.");
					}
					else
					{
						SendClientMessage(playerid, Vermelho, "Chegue mais perto para dar voz de prisao.");
					}
				}
				else
				{
					SendClientMessage(playerid, Vermelho, "Não pode parar uma pessoa que não tenha cometido crimes.");
				}
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Apenas policiais podem ultilizar este comando.");
		}
		return 1;
	}

	if(strcmp(cmd, "/algemar", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Policial_M || dini_Int(file, "Profissao") == Policial_C || dini_Int(file, "Profissao") == Policial_F || dini_Int(file, "Profissao") == Delegado || dini_Int(file, "Profissao") == Bope || dini_Int(file, "Profissao") == Swat || dini_Int(file, "Profissao") == Narcoticos || dini_Int(file, "Profissao") == FBI || dini_Int(file, "Profissao") == Interpol ||dini_Int(file, "Profissao") == LSPD || dini_Int(file, "aAdmin") == 1)
		{
			new plid;

			if(sscanf(cmdtext, "s[9]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "Use: /algemar [id]");
				return 1;
			}
			if(IsPlayerInAnyVehicle(plid) || IsPlayerInAnyVehicle(playerid))
			{
				SendClientMessage(playerid, Vermelho, "Alguem está dentro de um carro.");
				return 1 ;
			}
			if(plid == playerid)
			{
				SendClientMessage(playerid, Vermelho, "Você não pode algemar você mesmo.");
				return 1 ;
			}
			if(!IsPlayerConnected(plid))
			{
				SendClientMessage(playerid, Vermelho, "O(A) jogador(a) não está conectado.");
			}
			else
			{
				if(Procurados[plid] == 1 || infratores[plid] == 1)
				{
					if(GetDistanceBetweenPlayers(plid, playerid) < 10)
					{
						algemado[plid] = 1;
						TogglePlayerControllable(plid, 0);

						format(string, sizeof(string), "%s te algemou.", GetPlayerNameEx(playerid));
						SendClientMessage(plid, Blue, string);

						SendClientMessage(playerid, COLOR_GREEN, "Algemado com sucesso.");
					}
					else
					{
						SendClientMessage(playerid, Vermelho, "Chegue mais perto para algemar.");
					}
				}
				else
				{
					SendClientMessage(playerid, Vermelho, "Não pode algemar uma pessoa que não tenha cometido crimes.");
				}
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Apenas policiais podem usar este comando.");
		}
		return 1;
	}

	if(strcmp(cmd, "/desalgemar", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Policial_M || dini_Int(file, "Profissao") == Policial_C || dini_Int(file, "Profissao") == Policial_F || dini_Int(file, "Profissao") == Delegado || dini_Int(file, "Profissao") == Bope || dini_Int(file, "Profissao") == Swat || dini_Int(file, "Profissao") == Narcoticos || dini_Int(file, "Profissao") == FBI || dini_Int(file, "Profissao") == Interpol || dini_Int(file, "Profissao") == LSPD || dini_Int(file, "aAdmin") == 1)
		{
			new plid;

			if(sscanf(cmdtext, "s[12]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "Use: /desalgemar [id]");
				return 1;
			}
			if(IsPlayerInAnyVehicle(plid) || IsPlayerInAnyVehicle(playerid))
			{
				SendClientMessage(playerid, Vermelho, "Alguem está dentro de um carro.");
				return 1 ;
			}
			if(plid == playerid)
			{
				SendClientMessage(playerid, Vermelho, "Você não pode tirar algemas de você mesmo.");
				return 1 ;
			}
			if(!IsPlayerConnected(plid))
			{
				SendClientMessage(playerid, Vermelho, "O(A) jogador(a) não está conectado.");
			}
			else
			{
				if(algemado[plid] == 1)
				{
					if(GetDistanceBetweenPlayers(plid, playerid) < 10)
					{
						algemado[plid] = 0;
						TogglePlayerControllable(plid, 1);

						format(string, sizeof(string), "%s te tirou a algema.", GetPlayerNameEx(playerid));
						SendClientMessage(plid, Blue, string);

						SendClientMessage(playerid, COLOR_GREEN, "Desalgemado com sucesso.");
					}
					else
					{
						SendClientMessage(playerid, Vermelho, "Chegue mais perto para desalgemar.");
					}
				}
				else
				{
					SendClientMessage(playerid, Vermelho, "Essa pessoa não está algemada.");
				}
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Apenas policiais podem usar este comando.");
		}
		return 1;
	}

	if(strcmp(cmd, "/prender", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Policial_C || dini_Int(file, "Profissao") == Policial_F || dini_Int(file, "Profissao") == Delegado || dini_Int(file, "Profissao") == Bope || dini_Int(file, "Profissao") == Swat || dini_Int(file, "Profissao") == Narcoticos || dini_Int(file, "Profissao") == Interpol || dini_Int(file, "Profissao") == Policial_M || dini_Int(file, "Profissao") == FBI || dini_Int(file, "Profissao") == LSPD || dini_Int(file, "aAdmin") == 1)
		{
			new plid;

			if(sscanf(cmdtext, "s[9]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "Use: /prender [id]");
				return 1;
			}
			if(IsPlayerInAnyVehicle(plid) || IsPlayerInAnyVehicle(playerid))
			{
				SendClientMessage(playerid, Vermelho, "Alguem está dentro de um carro.");
				return 1 ;
			}
			if(plid == playerid)
			{
				SendClientMessage(playerid, Vermelho, "Você não pode fazer isto com si mesmo.");
				return 1 ;
			}
			if(!IsPlayerConnected(plid))
			{
				SendClientMessage(playerid, Vermelho, "O(A) jogador(a) não está conectado.");
			}
			else
			{
				if(GetDistanceBetweenPlayers(plid, playerid) < 10)
				{
					if(Procurados[plid] == 1)
					{
						PrenderPlayer(plid);
						GivePlayerGrana(playerid, 5000);

						format(string, sizeof(string), "%s te prendeu. Você estava sendo procurado(a).", GetPlayerNameEx(playerid));
						SendClientMessage(plid, Blue, string);

						SendClientMessage(playerid, Verde, "Você prendeu um(a) jogador(a) procurado(a) e ganhou 5 mil!");
					}
					else
					{
						SendClientMessage(playerid, Vermelho, "Você não pode prender alguem que não esteja sendo procurado!");
					}
				}
				else
				{
					SendClientMessage(playerid, Vermelho, "Chegue mais perto para prender!");
				}
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Apenas policiais podem usar este comando!");
		}
		return 1;
	}

	if(strcmp(cmd, "/venderak", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == TraficanteD || dini_Int(file, "Profissao") == YKZ || dini_Int(file, "Profissao") == Mafia || dini_Int(file, "aAdmin") == 1 || PlayerInfo[playerid][SCON] == true)
		{
			new plid;

			if(sscanf(cmdtext, "s[10]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "/venderak [id]");
				return 1;
			}
			if(IsPlayerInAnyVehicle(plid) || IsPlayerInAnyVehicle(playerid))
			{
				SendClientMessage(playerid, Vermelho, "Alguem está dentro de um carro.");
				return 1 ;
			}
			if(plid == playerid)
			{
				SendClientMessage(playerid, Vermelho, "Você não pode vender uma arma para você mesmo.");
				return 1 ;
			}
			if(!IsPlayerConnected(plid))
			{
				SendClientMessage(playerid, Vermelho, "O(A) jogador(a) não está conectado.");
			}
			else
			{
				if(GetDistanceBetweenPlayers(plid, playerid) < 10)
				{
					GivePlayerWeapon(plid, 30, 150);

					format(string, sizeof(string), "Um traficante de armas %s te vendeu uma AK-47, não faça DM fora da Favela.", GetPlayerNameEx(playerid));
					SendClientMessage(plid, Vermelho, string);

					SendClientMessage(playerid, COLOR_GREEN, "Ak 47 Vendida!");
				}
				else
				{
					SendClientMessage(playerid, Vermelho, "Chegue mais perto para Vender!");
				}
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Apenas traficantes de armas podem usar este comando!");
		}
		return 1;
	}

	if(strcmp(cmd, "/vendersniper", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == TraficanteD || dini_Int(file, "Profissao") == YKZ || dini_Int(file, "Profissao") == Mafia || dini_Int(file, "aAdmin") == 1 || PlayerInfo[playerid][SCON] == true)
		{
			new plid;

			if(sscanf(cmdtext, "s[14]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "/vendersniper [id]");
				return 1;
			}
			if(IsPlayerInAnyVehicle(plid) || IsPlayerInAnyVehicle(playerid))
			{
				SendClientMessage(playerid, Vermelho, "Alguem está dentro de um carro.");
				return 1 ;
			}
			if(plid == playerid)
			{
				SendClientMessage(playerid, Vermelho, "Você não pode vender uma arma para você mesmo.");
				return 1 ;
			}
			if(!IsPlayerConnected(plid))
			{
				SendClientMessage(playerid, Vermelho, "O(A) jogador(a) não está conectado.");
			}
			else
			{
				if(GetDistanceBetweenPlayers(plid, playerid) < 10)
				{
					GivePlayerWeapon(plid, 34, 25);

					format(string, sizeof(string), "Um traficante de armas %s te vendeu uma Sniper, não faça DM fora da Favela.", GetPlayerNameEx(playerid));
					SendClientMessage(plid, Vermelho, string);

					SendClientMessage(playerid, COLOR_GREEN, "Sniper Vendida!");
				}
				else
				{
					SendClientMessage(playerid, Vermelho, "Chegue mais perto para vender uma Sniper!");
				}
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Apenas traficantes de armas podem usar este comando!");
		}
		return 1;
	}

	if(strcmp(cmd, "/venderm4", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == TraficanteD || dini_Int(file, "Profissao") == YKZ || dini_Int(file, "Profissao") == Mafia || dini_Int(file, "aAdmin") == 1 || PlayerInfo[playerid][SCON] == true)
		{
			new plid;

			if(sscanf(cmdtext, "s[10]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "/venderm4 [id]");
				return 1;
			}
			if(IsPlayerInAnyVehicle(plid) || IsPlayerInAnyVehicle(playerid))
			{
				SendClientMessage(playerid, Vermelho, "Alguem está dentro de um carro.");
				return 1 ;
			}
			if(plid == playerid)
			{
				SendClientMessage(playerid, Vermelho, "Você não pode vender uma arma para você mesmo.");
				return 1 ;
			}
			if(!IsPlayerConnected(plid))
			{
				SendClientMessage(playerid, Vermelho, "O(A) jogador(a) não está conectado.");
			}
			else
			{
				if(GetDistanceBetweenPlayers(plid, playerid) < 10)
				{
					GivePlayerWeapon(plid, 31, 150);

					format(string, sizeof(string), "Um traficante de armas %s te vendeu uma M4, não faça DM fora da Favela.", GetPlayerNameEx(playerid));
					SendClientMessage(plid, Vermelho, string);

					SendClientMessage(playerid, COLOR_GREEN, "M4 Vendida!");
				}
				else
				{
					SendClientMessage(playerid, Vermelho, "Chegue mais perto para vender um M4!");
				}
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Apenas traficantes de armas podem usar este comando!");
		}
		return 1;
	}

	if(strcmp(cmd, "/venderswanoff", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == TraficanteD || dini_Int(file, "Profissao") == YKZ || dini_Int(file, "Profissao") == Mafia || dini_Int(file, "aAdmin") == 1 || PlayerInfo[playerid][SCON] == true)
		{
			new plid;

			if(sscanf(cmdtext, "s[15]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "/venderswanoff [id]");
				return 1;
			}
			if(IsPlayerInAnyVehicle(plid) || IsPlayerInAnyVehicle(playerid))
			{
				SendClientMessage(playerid, Vermelho, "Alguem está dentro de um carro.");
				return 1 ;
			}
			if(plid == playerid)
			{
				SendClientMessage(playerid, Vermelho, "Você não pode vender uma arma para você mesmo.");
				return 1 ;
			}
			if(!IsPlayerConnected(plid))
			{
				SendClientMessage(playerid, Vermelho, "O(A) jogador(a) não está conectado.");
			}
			else
			{
				if(GetDistanceBetweenPlayers(plid, playerid) < 10)
				{
					GivePlayerWeapon(plid, 26, 35);

					format(string, sizeof(string), "Um traficante de armas %s te vendeu uma Swan Off, não faça DM fora da Favela.", GetPlayerNameEx(playerid));
					SendClientMessage(plid, Vermelho, string);

					SendClientMessage(playerid, COLOR_GREEN, "Swan Off Vendida!");
				}
				else
				{
					SendClientMessage(playerid, Vermelho, "Chegue mais perto para vender uma Swan Off!");
				}
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Apenas traficantes de armas podem usar este comando!");
		}
		return 1;
	}

	if(strcmp(cmd, "/vendertec", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == TraficanteD || dini_Int(file, "Profissao") == YKZ || dini_Int(file, "Profissao") == Mafia || dini_Int(file, "aAdmin") == 1 || PlayerInfo[playerid][SCON] == true)
		{
			new plid;

			if(sscanf(cmdtext, "s[11]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "/vendertec [id]");
				return 1;
			}
			if(IsPlayerInAnyVehicle(plid) || IsPlayerInAnyVehicle(playerid))
			{
				SendClientMessage(playerid, Vermelho, "Alguem está dentro de um carro.");
				return 1;
			}
			if(plid == playerid)
			{
				SendClientMessage(playerid, Vermelho, "Você não pode fazer isto com si mesmo.");
				return 1;
			}
			if(!IsPlayerConnected(plid))
			{
				SendClientMessage(playerid, Vermelho, "O(A) jogador(a) não está conectado.");
			}
			else
			{
				if(GetDistanceBetweenPlayers(plid, playerid) < 10)
				{
					GivePlayerWeapon(plid, 32, 150);

					format(string, sizeof(string), "Um traficante de armas %s te vendeu uma Tec, não faça DM fora da Favela.", GetPlayerNameEx(playerid));
					SendClientMessage(plid, Vermelho, string);

					SendClientMessage(playerid, COLOR_GREEN, "Tec Vendida!");
				}
				else
				{
					SendClientMessage(playerid, Vermelho, "Chegue mais perto para vender uma Tec!");
				}
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Apenas traficantes de armas podem usar este comando!");
		}
		return 1;
	}

	if(strcmp(cmd, "/transferir", true) == 0)
	{
		new plid, quantia;

		if(sscanf(cmdtext, "s[12]ud", cmd, plid, quantia))
		{
			SendClientMessage(playerid, Vermelho, "Use: /transferir [id] [quantidade]");
			return 1;
		}
		if(IsPlayerConnected(plid))
		{
			if(quantia > 0 && GetPlayerGrana(playerid) >= quantia)
			{
				GivePlayerGrana(playerid, -quantia);
				GivePlayerGrana(plid, quantia);

				format(string, sizeof(string), "Você transferiu para %s (ID: %d) a importância de $%d.", GetPlayerNameEx(plid), plid, quantia);
				SendClientMessage(playerid, Amarelo, string);

				format(string, sizeof(string), "Você recebeu $%d de %s (ID: %d).", quantia, GetPlayerNameEx(playerid), playerid);
				SendClientMessage(plid, Amarelo, string);
			}
			else
			{
				SendClientMessage(playerid, Amarelo, "Valor inválido.");
			}
		}
		else
		{
			SendClientMessage(playerid, Amarelo, "ID inválido, tente novamente.");
		}
		return 1;
	}

	if(strcmp(cmd, "/avisar", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Sequestrador || dini_Int(file, "aAdmin") == 1 || PlayerInfo[playerid][SCON] == true)
		{
			new texto[128], msg[128];

			if(sscanf(cmdtext, "s[8]s[128]", cmd, texto))
			{
				SendClientMessage(playerid, Vermelho, "/avisar [texto]");
			}
			else
			{
				format(msg, sizeof(msg), "%s avisa: %s", GetPlayerNameEx(playerid), texto);
				SendClientMessageToAll(C_Sequestrador, msg);
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp(cmd, "/esmola", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Mendigo)
		{
			new msg[128];

			format(msg, sizeof(msg), "%s diz: Sou morador de rua, o senhor pode me dar um dinheirinho pra comprar pão?", GetPlayerNameEx(playerid));
			SendClientMessageToAll(C_Sequestrador, msg);
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp(cmd, "/vigiar", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Flanelinha)
		{
			new msg[128];

			format(msg, sizeof(msg), "%s diz: Vigio carros por $50, pague após o serviço.", GetPlayerNameEx(playerid));
			SendClientMessageToAll(C_Sequestrador, msg);
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp(cmd, "/rimar", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Rapper  || dini_Int(file, "aAdmin") == 1 || PlayerInfo[playerid][SCON] == true)
		{
			new texto[128], msg[128];

			if(sscanf(cmdtext, "s[7]s[128]", cmd, texto))
			{
				SendClientMessage(playerid, Vermelho, "Use: /rimar [texto]");
			}
			else
			{
				format(msg, sizeof(msg), "%s faz uma rima: %s", GetPlayerNameEx(playerid), texto);
				SendClientMessageToAll(C_Rapper, msg);
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp(cmd, "/cantar", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Cantor)
		{
			new texto[128], msg[128];

			if(sscanf(cmdtext, "s[8]s[128]", cmd, texto))
			{
				SendClientMessage(playerid, Vermelho, "Use: /cantar [texto]");
			}
			else
			{
				format(msg, sizeof(msg), "%s canta: %s", GetPlayerNameEx(playerid), texto);
				SendClientMessageToAll(C_Cantor, msg);
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp(cmd, "/poema", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Poeta)
		{
			new texto[128], msg[128];

			if(sscanf(cmdtext, "s[7]s[128]", cmd, texto))
			{
				SendClientMessage(playerid, Vermelho, "/poeta [poema]");
			}
			else
			{
				format(msg, sizeof(msg), "%s recita o poema: %s", GetPlayerNameEx(playerid), texto);
				SendClientMessageToAll(C_Rapper, msg);
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp(cmd, "/corrida", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Corredor  || dini_Int(file, "aAdmin") == 1 || PlayerInfo[playerid][SCON] == true)
		{
			new texto[128], msg[128];

			if(sscanf(cmdtext, "s[9]s[128]", cmd, texto))
			{
				SendClientMessage(playerid, Vermelho, "/corrida [texto]");
			}
			else
			{
				format(msg, sizeof(msg), "%s marca um racha: %s", GetPlayerNameEx(playerid), texto);
				SendClientMessageToAll(C_Corredor, msg);
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp(cmd, "/dk", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Drifter  || dini_Int(file, "aAdmin") == 1 || PlayerInfo[playerid][SCON] == true)
		{
			new texto[128], msg[128];

			if(sscanf(cmdtext, "s[3]s[128]", cmd, texto))
			{
				SendClientMessage(playerid, Vermelho, "/dk [texto]");
			}
			else
			{
				format(msg, sizeof(msg), "%s anuncia: %s", GetPlayerNameEx(playerid), texto);
				SendClientMessageToAll(C_Rapper, msg);
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp(cmd, "/na", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Ninja  || dini_Int(file, "aAdmin") == 1 || PlayerInfo[playerid][SCON] == true)
		{
			new texto[128], msg[128];

			if(sscanf(cmdtext, "s[3]s[128]", cmd, texto))
			{
				SendClientMessage(playerid, Vermelho, "/na [texto]");
			}
			else
			{
				format(msg, sizeof(msg), "%s anuncia: %s", GetPlayerNameEx(playerid), texto);
				SendClientMessageToAll(C_Ninja, msg);
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp(cmd, "/noticiar", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Jornalista || dini_Int(file, "Profissao") == Reporter || dini_Int(file, "Profissao") == Ancora || dini_Int(file, "aAdmin") == 1 || PlayerInfo[playerid][SCON] == true)
		{
			new texto[128], msg[128];

			if(sscanf(cmdtext, "s[10]s[128]", cmd, texto))
			{
				SendClientMessage(playerid, Vermelho, "/noticiar [texto]");
			}
			else
			{
				format(msg, sizeof(msg), "%s publica uma notícia: %s", GetPlayerNameEx(playerid), texto);
				SendClientMessageToAll(C_Reporter, msg);
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp(cmd, "/publicar", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Correio)
		{
			new texto[128], msg[128];

			if(sscanf(cmdtext, "s[10]s[128]", cmd, texto))
			{
				SendClientMessage(playerid, Vermelho, "/publicar [texto]");
			}
			else
			{
				format(msg,sizeof(msg), "%s publica uma carta: %s", GetPlayerNameEx(playerid), texto);
				SendClientMessageToAll(C_Reporter, msg);
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp(cmd, "/discurso", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Prefeito || PlayerInfo[playerid][SCON] == true)
		{
			new texto[128], msg[128];

			if(sscanf(cmdtext, "s[10]s[128]", cmd, texto))
			{
				SendClientMessage(playerid, Vermelho, "/discurso [texto]");
			}
			else
			{
				format(msg, sizeof(msg), "%s discursa: %s", GetPlayerNameEx(playerid), texto);
				SendClientMessageToAll(C_Prefeito, msg);
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp(cmd, "/discursar", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Presidente || PlayerInfo[playerid][SCON] == true)
		{
			new texto[128], msg[128];

			if(sscanf(cmdtext, "s[11]s[128]", cmd, texto))
			{
				SendClientMessage(playerid, Vermelho, "/discursar [texto]");
			}
			else
			{
				format(msg, sizeof(msg), "O presidente %s discursa: %s", GetPlayerNameEx(playerid), texto);
				SendClientMessageToAll(C_Presidente, msg);
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp(cmd, "/terror", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Terrorista || dini_Int(file, "aAdmin") == 1 || PlayerInfo[playerid][SCON] == true)
		{
			new texto[128], msg[128];

			if(sscanf(cmdtext, "s[8]s[128]", cmd, texto))
			{
				SendClientMessage(playerid, Vermelho, "/terror [texto]");
			}
			else
			{
				format(msg, sizeof(msg), "%s ameaça: %s", GetPlayerNameEx(playerid), texto);
				SendClientMessageToAll(C_Terrorista, msg);
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
	}

	if(strcmp(cmd, "/festa", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Promoter || dini_Int(file, "aAdmin") == 1 || PlayerInfo[playerid][SCON] == true)
		{
			new texto[128], msg[128];

			if(sscanf(cmdtext, "s[7]s[128]", cmd, texto))
			{
				SendClientMessage(playerid, Vermelho, "/festa [texto]");
			}
			else
			{
				format(msg, sizeof(msg), "%s anuncia uma festa: %s", GetPlayerNameEx(playerid), texto);
				SendClientMessageToAll(C_Promoter, msg);
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp(cmd, "/reparar", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Mecanico || dini_Int(file, "aAdmin") == 1 || PlayerInfo[playerid][SCON] == true)
		{
			if(IsPlayerInAnyVehicle(playerid))
			{
				RepairVehicle(GetPlayerVehicleID(playerid));
				UpdateBar(playerid);

				SendClientMessage(playerid, Blue, "Você consertou um carro, cobre pelo serviço.");
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Você não está em um veículo para consertar.");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/limparcasa", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Empregada)
		{
			SendClientMessage(playerid, Blue, "Você limpou a casa, cobre pelo serviço.");
		}
		return 1;
	}

	if(strcmp(cmd, "/catarlixo", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Lixeiro || dini_Int(file, "Profissao") == Gari)
		{
			SendClientMessage(playerid, Blue, "Você limpou a rua, você receberá o extra no salário.");
		}
		return 1;
	}

	if(strcmp(cmd, "/reformar", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Pedreiro)
		{
			SendClientMessage(playerid, Blue, "Você reformou o objeto, cobre pelo serviço.");
		}
		return 1;
	}

	if(strcmp(cmd, "/vcontrole", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Mecanico || dini_Int(file, "aAdmin") == 1 || PlayerInfo[playerid][SCON] == true)
		{
			#if defined VControleUser
			if(!IsPlayerInAnyVehicle(playerid))
			{
				SendClientMessage(playerid, 0xCC0000FF, "Você precisa estar em um veículo para usar este comando.");
			}
			else
			{
				ShowPlayerDialog(playerid, vcontrole, DIALOG_STYLE_LIST, "Controle", "{FF0000}Luzes\n{0800FF}Abrir/Fechar Capo\n{00FF08}Abrir/Fechar Boot\n{FFFF00}Motor\n{AE00FF}Alarme", "Confirmar", "Cancelar");
			}
			#else
			SendClientMessage(playerid, LARANJA, "Comando desativado temporáriamente.");
			#endif
		}
		return 1;
	}

	if(strcmp("/tunar", cmd, true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(IsPlayerVIP(playerid) || dini_Int(file, "Profissao") == Mecanico || dini_Int(file, "aAdmin") == 1 || PlayerInfo[playerid][SCON] == true)
		{
			if(!IsPlayerInAnyVehicle(playerid))
			{
				SendClientMessage(playerid, 0xCC0000FF, "Você precisa estar em um veículo para usar este comando.");
			}
			else
			{
				ShowPlayerDialog(playerid, tunar, DIALOG_STYLE_LIST, "Tuning Menu", STRD_TUNNING, "OK", "Cancelar");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/pintar", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Mecanico || dini_Int(file, "aAdmin") == 1 || PlayerInfo[playerid][SCON] == true)
		{
			new cor1, cor2;

			if(sscanf(cmdtext, "s[8]dd", cmd, cor1, cor2))
			{
				SendClientMessage(playerid, Vermelho, "/pintar [cor1] [cor2]");
				return 1;
			}
			if(IsPlayerInVehicle(playerid, GetPlayerVehicleID(playerid)))
			{
				ChangeVehicleColor(GetPlayerVehicleID(playerid), cor1, cor2);
				SendClientMessage(playerid, Blue, "Você pintou o carro, cobre pelo serviço.");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/darterrestre", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "aAdmin") == 1 || PlayerInfo[playerid][SCON] == true)
		{
			new plid;

			if(sscanf(cmdtext, "s[14]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /darterrestre [id]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				format(file2, sizeof(file2), PASTA_CONTAS, GetPlayerNameEx(plid));
				dini_IntSet(file2, "HabTerrestre", 1);

				format(string, sizeof(string), "Você deu uma carteira terrestre para %s.", GetPlayerNameEx(plid));
				SendClientMessage(playerid, Blue, string);

				format(string, sizeof(string), "%s (%d) te deu uma carteira terrestre.", GetPlayerNameEx(playerid), playerid);
				SendClientMessage(plid, tcadm, string);
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente!");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/daraerea", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "aAdmin") == 1 || PlayerInfo[playerid][SCON] == true)
		{
			new plid;

			if(sscanf(cmdtext, "s[10]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /daraerea [id]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				format(file2, sizeof(file2), PASTA_CONTAS, GetPlayerNameEx(plid));
				dini_IntSet(file2, "HabAerea", 1);

				format(string, sizeof(string), "Você deu uma carteira aérea para %s.", GetPlayerNameEx(plid));
				SendClientMessage(playerid, Blue, string);

				format(string, sizeof(string), "%s (%d) te deu uma carteira aérea.", GetPlayerNameEx(playerid), playerid);
				SendClientMessage(plid, tcadm, string);
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente!");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/darnautica", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "aAdmin") == 1 || PlayerInfo[playerid][SCON] == true)
		{
			new plid;

			if(sscanf(cmdtext, "s[12]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /darnautica [id]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				format(file2, sizeof(file2), PASTA_CONTAS, GetPlayerNameEx(plid));
				dini_IntSet(file2, "HabNautica", 1);

				format(string, sizeof(string), "Você deu uma carteira náutica para %s.", GetPlayerNameEx(plid));
				SendClientMessage(playerid, Blue, string);

				format(string, sizeof(string), "%s (%d) te deu uma carteira náutica.", GetPlayerNameEx(playerid), playerid);
				SendClientMessage(plid, tcadm, string);
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente!");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/darcarteira", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "aAdmin") == 1 || PlayerInfo[playerid][SCON] == true)
		{
			new plid;

			if(sscanf(cmdtext, "s[13]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /darcarteira [id]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				format(file2, sizeof(file2), PASTA_CONTAS, GetPlayerNameEx(plid));
				dini_IntSet(file2, "Carteira", 1);

				format(string, sizeof(string), "Você deu uma carteira de trabalho para %s.", GetPlayerNameEx(plid));
				SendClientMessage(playerid, Blue, string);

				format(string, sizeof(string), "%s (%d) te deu uma carteira de trabalho.", GetPlayerNameEx(playerid), playerid);
				SendClientMessage(plid, tcadm, string);
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente!");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/darporte", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "aAdmin") == 1 || PlayerInfo[playerid][SCON] == true)
		{
			new plid;

			if(sscanf(cmdtext, "s[10]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /darporte [id]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				format(file2, sizeof(file2), PASTA_CONTAS, GetPlayerNameEx(plid));
				dini_IntSet(file2, "Porte", 1);

				format(string, sizeof(string), "Você deu porte de armas para %s.", GetPlayerNameEx(plid));
				SendClientMessage(playerid, Blue, string);

				format(string, sizeof(string), "%s (%d) te deu porte de armas.", GetPlayerNameEx(playerid), playerid);
				SendClientMessage(plid, tcadm, string);
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente!");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/venderskin", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == VendedorSkin || dini_Int(file, "aAdmin") == 1 || PlayerInfo[playerid][SCON] == true)
		{
			new plid, skin;

			if(sscanf(cmdtext, "s[12]ud", cmd, plid, skin))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /venderskin [id] [skin]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				if(skin >= 0 && skin < 299)
				{
					if(skin == 292 || skin == 271 || skin == 272 || skin == 273 || skin == 270 || skin == 269 || skin == 274 || skin == 289)
					{
						SendClientMessage(playerid, Vermelho, "Skin proibido, tente outro! | ID's = 0-298");
						return 1;
					}
					format(file2, sizeof(file2), PASTA_CONTAS, GetPlayerNameEx(plid));

					SetPlayerSkin(plid, skin);
					dini_IntSet(file2, "Skin", skin);

					format(string, sizeof(string), "%s (ID: %d) te vendeu uma nova roupa ID: %d", GetPlayerNameEx(playerid), playerid, skin);
					SendClientMessage(plid, Blue, string);

					format(string, sizeof(string), "Você vendeu ao %s (ID: %d) a roupa ID: %d", GetPlayerNameEx(plid), plid, skin);
					SendClientMessage(playerid, Blue, string);
				}
				else
				{
					SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente! | ID's = 0-298");
				}
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Jogador não conectado!");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/dargrana", true) == 0)
	{
		if(pAdmin[playerid] > 3)
		{
			new plid, grana;

			if(sscanf(cmdtext, "s[10]ud", cmd, plid, grana))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /dargrana [id] [grana]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				GivePlayerGrana(plid, grana);

				format(string, sizeof(string), "Você deu $%d para %s.", grana, GetPlayerNameEx(plid));
				SendClientMessage(playerid, Blue, string);

				format(string, sizeof(string), "%s (%d) te deu $%d, não gaste com doces.", GetPlayerNameEx(playerid), playerid, grana);
				SendClientMessage(plid, tcadm, string);
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Jogador não conectado!");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/lembrete", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Bloco") == 1)
		{
			new texto[128];

			if(sscanf(cmdtext, "s[10]s[128]", cmd, texto))
			{
				SendClientMessage(playerid, COLOR_GREEN, "Use /lembrete [texto]");
				return 1;
			}
			if(IsPlayerConnected(playerid))
			{
				dini_Set(file, "Lembrete", texto);
				SendClientMessage(playerid, 0xFF0000AA, "Lembrete anotado!");
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Você não tem um bloco de lembretes.");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/darcomb", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Frentista || PlayerInfo[playerid][SCON] == true || pAdmin[playerid] > 3)
		{
			new msg[256], petroleo, plid, comb;

			if(sscanf(cmdtext, "s[9]ud", cmd, plid, comb))
			{
				SendClientMessage(playerid, Vermelho, "/darcomb [id] [quantidade]");
				return 1;
			}
			if(comb > MAX_COMB || comb <= 0)
			{
				SendClientMessage(playerid, Vermelho, "Quantidade inválida. Os valores devem ser de 1 à "#MAX_COMB".");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				if(IsPlayerInAnyVehicle(plid) == 1)
				{
					for(new carro = 0; carro < MAX_CONCES; carro++)
					{
						format(file2, sizeof(file2), PASTA_CONCE, carro);
						if(dini_Exists(file2))
						{
							if(GetPlayerVehicleID(plid) == dini_Int(file2, "Id"))
							{
								if((dini_Int(file2, "Combustivel")+comb) < MAX_COMB)
								{
									petroleo = dini_Int(file2, "Combustivel")+comb;
									dini_IntSet(file2, "Combustivel", petroleo);

									new engine, lights, alarm, doors, bonnet, boot, objective;
									GetVehicleParamsEx(dini_Int(file2, "Id"), engine, lights, alarm, doors, bonnet, boot, objective);
									SetVehicleParamsEx(dini_Int(file2, "Id"), VEHICLE_PARAMS_ON, lights, alarm, doors, bonnet, boot, objective);

									format(msg, sizeof(msg), "Um frentista colocou %d L de combustível em seu tanque. Total: %d L", comb, petroleo);
									SendClientMessage(plid, COLOR_GREEN, msg);

									SendClientMessage(playerid, COLOR_GREEN, "Combustível fornecido com sucesso!");
								}
								else if((dini_Int(file2, "Combustivel")+comb) >= MAX_COMB)
								{
									petroleo = MAX_COMB-dini_Int(file2, "Combustivel");
									dini_IntSet(file2, "Combustivel", MAX_COMB);

									new engine, lights, alarm, doors, bonnet, boot, objective;
									GetVehicleParamsEx(dini_Int(file2, "Id"), engine, lights, alarm, doors, bonnet, boot, objective);
									SetVehicleParamsEx(dini_Int(file2, "Id"), VEHICLE_PARAMS_ON, lights, alarm, doors, bonnet, boot, objective);

									format(msg, sizeof(msg), "Um frentista completou seu tanque colocando %d L.", petroleo);
									SendClientMessage(plid, COLOR_GREEN, msg);

									SendClientMessage(playerid, COLOR_GREEN, "Combustível fornecido com sucesso!");
								}
								return 1;
							}
						}
					}
					format(string, sizeof(string), "%s não está em um veículo da Conce.", GetPlayerNameEx(plid));
					SendClientMessage(playerid, COLOR_RED, string);
				}
				else
				{
					format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(plid));
					if((dini_Int(file, "Combustivel")+comb) < MAX_COMB)
					{
						petroleo = dini_Int(file, "Combustivel")+comb;
						dini_IntSet(file, "Combustivel", petroleo);

						format(msg, sizeof(msg), "Um frentista colocou %d L de combustível em seus galões. Total: %d L", comb, petroleo);
						SendClientMessage(plid, COLOR_GREEN, msg);

						SendClientMessage(playerid, COLOR_GREEN, "Combustível fornecido com sucesso!");
					}
					else if((dini_Int(file, "Combustivel")+comb) >= MAX_COMB)
					{
						petroleo = MAX_COMB-dini_Int(file, "Combustivel");
						dini_IntSet(file, "Combustivel", MAX_COMB);

						format(msg, sizeof(msg), "Um frentista completou seus galões colocando %d L.", petroleo);
						SendClientMessage(plid, COLOR_GREEN, msg);

						SendClientMessage(playerid, COLOR_GREEN, "Combustível fornecido com sucesso!");
					}
					ShowPlayerDialog(plid, playersimp, DIALOG_STYLE_MSGBOX, "Frentistas", "{FFFF00}Você não está em um veículo.\n{FFFFFF}Enchemos galões de combustível para você.\n{FF33FF}Vá até seu carro e use /enchertanque para colocar este combustível.", "OK", "");
				}
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Jogador não conectado/logado.");
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp(cmd, "/darlevel", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "aAdmin") == 1 || PlayerInfo[playerid][SCON] == true)
		{
			new plid, level;

			if(sscanf(cmdtext, "s[10]ud", cmd, plid, level))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /darlevel [id] [level]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				if(level < MAX_PLAYER_LEVEL-1)
				{
					format(file2, sizeof(file2), PASTA_CONTAS, GetPlayerNameEx(plid));
					if(dini_Int(file2, "Level") < MAX_PLAYER_LEVEL-1)
					{
						dini_IntSet(file2, "Level", dini_Int(file2, "Level")+level);

						format(string, sizeof(string), "%s (%d) te deu %d level(s).", GetPlayerNameEx(playerid), playerid, level);
						SendClientMessage(plid, tcadm, string);

						format(string, sizeof(string), "Você deu a %s (ID: %d) %d level(s).", GetPlayerNameEx(plid), plid, level);
						SendClientMessage(playerid, Blue, string);
					}
					else
					{
						SendClientMessage(playerid, Vermelho, "Este jogador já alcançou o limite de level(s).");
					}
				}
				else
				{
					format(string, sizeof(string), "Não é permitido dar mais de %d level(s).", MAX_PLAYER_LEVEL);
					SendClientMessage(playerid, Vermelho, string);
				}
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Jogador não conectado!");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/setlevel", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "aAdmin") == 1 || PlayerInfo[playerid][SCON] == true)
		{
			new plid, level;

			if(sscanf(cmdtext, "s[10]ud", cmd, plid, level))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /setlevel [id] [level]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				if(level < MAX_PLAYER_LEVEL-1)
				{
					format(file2, sizeof(file2), PASTA_CONTAS, GetPlayerNameEx(plid));
					if(dini_Int(file2, "Level") < MAX_PLAYER_LEVEL-1)
					{
						dini_IntSet(file2, "Level", level);

						format(string, sizeof(string), "%s (%d) setou a você %d level(s).", GetPlayerNameEx(playerid), playerid, level);
						SendClientMessage(plid, tcadm, string);

						format(string, sizeof(string), "Você setou a %s (ID: %d) %d level(s).", GetPlayerNameEx(plid), plid, level);
						SendClientMessage(playerid, Blue, string);
					}
					else
					{
						SendClientMessage(playerid, Vermelho, "Este jogador já alcançou o limite de level(s).");
					}
				}
				else
				{
					format(string, sizeof(string), "Não é permitido setar mais de %d level(s).", MAX_PLAYER_LEVEL);
					SendClientMessage(playerid, Vermelho, string);
				}
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Jogador não conectado!");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/darestudo", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Professor || dini_Int(file, "aAdmin") == 1 || PlayerInfo[playerid][SCON] == true)
		{
			new plid, estudo;

			if(sscanf(cmdtext, "s[10]ud", cmd, plid, estudo))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /darestudo [id] [estudo]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				if(estudo < MAX_PLAYER_ESTUDO-1)
				{
					format(file2, sizeof(file2), PASTA_CONTAS, GetPlayerNameEx(plid));
					if(dini_Int(file2, "Faculdade") < MAX_PLAYER_ESTUDO-1)
					{
						if(dini_Int(file, "Profissao") == Professor)
						{
							if(estudo > 5)
							{
								SendClientMessage(playerid, Blue, "Você não pode dar mais de 5 estudos de uma vez.");
								return 1;
							}
						}
						dini_IntSet(file2, "Faculdade", dini_Int(file2, "Faculdade")+estudo);

						format(string, sizeof(string), "%s (%d) te deu %d estudo(s).", GetPlayerNameEx(playerid), playerid, estudo);
						SendClientMessage(plid, tcadm, string);

						format(string, sizeof(string), "Você deu a %s (ID: %d) %d estudo(s).", GetPlayerNameEx(plid), plid, estudo);
						SendClientMessage(playerid, Blue, string);
					}
					else
					{
						SendClientMessage(playerid, Vermelho, "Este jogador já alcançou o limite de estudo(s).");
					}
				}
				else
				{
					format(string, sizeof(string), "Não é permitido dar mais de %d estudo(s).", MAX_PLAYER_ESTUDO);
					SendClientMessage(playerid, Vermelho, string);
				}
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Jogador não conectado!");
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não é um professor!");
		}
		return 1;
	}

	if(strcmp(cmd, "/setestudo", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "aAdmin") == 1 || PlayerInfo[playerid][SCON] == true)
		{
			new plid, estudo;

			if(sscanf(cmdtext, "s[11]ud", cmd, plid, estudo))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /setestudo [id] [level]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				if(estudo < MAX_PLAYER_ESTUDO-1)
				{
					format(file2, sizeof(file2), PASTA_CONTAS, GetPlayerNameEx(plid));
					if(dini_Int(file2, "Faculdade") < MAX_PLAYER_ESTUDO-1)
					{
						dini_IntSet(file2, "Faculdade", estudo);

						format(string, sizeof(string), "%s (%d) te setou %d estudo(s).", GetPlayerNameEx(playerid), playerid, estudo);
						SendClientMessage(plid, tcadm, string);

						format(string, sizeof(string), "Você setou a %s (ID: %d) %d estudo(s).", GetPlayerNameEx(plid), plid, estudo);
						SendClientMessage(playerid, Blue, string);
					}
					else
					{
						SendClientMessage(playerid, Vermelho, "Este jogador já alcançou o limite de estudo(s).");
					}
				}
				else
				{
					format(string, sizeof(string), "Não é permitido setar mais de %d estudo(s).", MAX_PLAYER_ESTUDO);
					SendClientMessage(playerid, Vermelho, string);
				}
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Jogador não conectado!");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/setprof", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Empregador || dini_Int(file, "aAdmin") == 1 || PlayerInfo[playerid][SCON] == true)
		{
			new plid, prof;

			if(sscanf(cmdtext, "s[9]ud", cmd, plid, prof))
			{
				SendClientMessage(playerid, Vermelho, "Digite: /setprof [id] [prof]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				if(prof == MOD)
				{
					if(pAdmin[playerid] != 5)
					{
						SendClientMessage(playerid, Vermelho, "Você não tem permissão para setar esta profissaõ.");
						return 1;
					}
				}
				format(file2, sizeof(file2), PASTA_CONTAS, GetPlayerNameEx(plid));
				dini_IntSet(file2, "Profissao", prof);
				SpawnPlayer(plid);

				format(string, sizeof(string), "%s (%d) te deu um emprego ID: %d", GetPlayerNameEx(playerid), playerid, prof);
				SendClientMessage(plid, tcadm, string);

				format(string, sizeof(string), "Você deu a %s (ID: %d) um emprego ID: %d", GetPlayerNameEx(plid), plid, prof);
				SendClientMessage(playerid, Blue, string);
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Jogador não conectado.");
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não é um empregador!");
		}
		return 1;
	}

	if(strcmp("/verlevel", cmd, true) == 0)
	{
		new strcmd[1000];

		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));

		format(string, sizeof(string), "{00FF00}» Tempo EXP: %d/%d     {CCFF00}» Seu Level: %d\n", dini_Int(file, "Tempo"), TEMPO_EXP, PlayerInfo[playerid][_Level]);
		strcat(strcmd, string, sizeof(strcmd));

		format(string, sizeof(string), "{00FF00}» Sua Experiência: %d  {CCFF00}» Seu Estudo: %d\n", PlayerInfo[playerid][_EXP], PlayerInfo[playerid][_Faculdade]);
		strcat(strcmd, string, sizeof(strcmd));

		ShowPlayerDialog(playerid, playersimp, DIALOG_STYLE_MSGBOX, ".:: - Level(s) - ::.", strcmd, "OK", "");
		return 1;
	}

	if(strcmp("/rg", cmd, true) == 0)
	{
		new string222[256];

		format(string222, sizeof(string222), "......:::::: - %s: [ID: %d] - ::::::.....", GetPlayerNameEx(playerid), playerid);
		SendClientMessage(playerid, -1, string222);

		format(string, sizeof(string), "{CCFF00}» EXP: %d/%d        {00FF00}» Grana na Mão: $%d", PlayerInfo[playerid][_EXP], MAX_PLAYER_EXP, GetPlayerGrana(playerid));
		SendClientMessage(playerid, -1, string);

		format(string, sizeof(string), "{CCFF00}» Level: %d/%d      {00FF00}» Saldo Bancário: $%d", PlayerInfo[playerid][_Level], MAX_PLAYER_LEVEL, PlayerInfo[playerid][_SaldoBancario]);
		SendClientMessage(playerid, -1, string);

		format(string, sizeof(string), "{CCFF00}» Estudo: %d/%d     {00FF00}» Casou Com: %s", PlayerInfo[playerid][_Faculdade], MAX_PLAYER_ESTUDO, PlayerInfo[playerid][_CasouCom]);
		SendClientMessage(playerid, -1, string);

		format(string, sizeof(string), "{CCFF00}» Profissão ID: %d  {00FF00}» Skin ID: %d", PlayerInfo[playerid][_Profissao], PlayerInfo[playerid][_Skin]);
		SendClientMessage(playerid, -1, string);

		format(string, sizeof(string), "{CCFF00}» Matou: %d         {00FF00}» Morreu: %d", matou[playerid], morreu[playerid]);
		SendClientMessage(playerid, -1, string);

		SendClientMessage(playerid, -1, string222);
		return 1;
	}

	if(strcmp(cmd, "/fakexat", true) == 0)
	{
		new msg[256], plid, texto[128];

		if(sscanf(cmdtext, "s[9]us[128]", cmd, plid, texto))
		{
			SendClientMessage(playerid, Vermelho, "Digite: /fakexat [id] [texto]");
			return 1;
		}
		if(pAdmin[playerid] == 5)
		{
			format(msg, sizeof(msg), "%s: {FFFFFF}[ID: %d] %s", GetPlayerNameEx(plid), plid, texto);
			SendClientMessageToAll(GetPlayerColor(plid), msg);
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão!");
		}
		return 1;
	}

	if(strcmp(cmd, "/saldocell", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Celular") == 1)
		{
			if(dini_Int(file, "ContaBancaria") == 1)
			{
				format(string, sizeof(string), "[TORPEDO]: Você tem depositado no banco $%d.", dini_Int(file, "SaldoBancario"));
				SendClientMessage(playerid, Verde, string);
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Você não tem uma conta bancária.");
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem celular.");
		}
		return 1;
	}

	if(strcmp(cmd, "/velocimetro", true) == 0)
	{
		if(IsPlayerInAnyVehicle(playerid))
		{
			SendClientMessage(playerid, Vermelho, "Saia do veículo para mudar o velocimetro.");
			return 1;
		}
		if(statuscp[playerid] == 0)
		{
			statuscp[playerid] = 1;

			#if defined gText1User
			PlayerTextDrawHide(playerid, PlayerInfo[playerid][gText1]);
			#endif

			SendClientMessage(playerid, COLOR_GREEN, "Você mudou o sistema de velocimetro.");
		}
		else
		{
			statuscp[playerid] = 0;

			#if defined gText1User
			PlayerTextDrawHide(playerid, PlayerInfo[playerid][gText1]);
			#endif

			SendClientMessage(playerid, COLOR_GREEN, "Você mudou o sistema de velocimetro.");
		}
		return 1;
	}

	if(strcmp("/sendcmd", cmd, true) == 0)
	{
		if(pAdmin[playerid] > 4)
		{
			new plid, comando[256];

			if(sscanf(cmdtext, "s[9]us[256]", cmd, plid, comando))
			{
				SendClientMessage(playerid, Vermelho, "Use /sendcmd [id] [comando]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				OnPlayerCommandText(plid, comando);
				SendClientMessage(playerid, COLOR_GREEN, "Comando enviado!");
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Jogador não conectado!");
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp("/status", cmd, true) == 0)
	{
		new plid;

		if(sscanf(cmdtext, "s[8]u", cmd, plid))
		{
			SendClientMessage(playerid, COLOR_GREEN, "Use /status [id]");
			return 1;
		}
		if(IsPlayerConnected(plid))
		{
			new string222[512];

			format(string222, sizeof(string222), "......:::::: - %s: [ID: %d] - ::::::.....", GetPlayerNameEx(plid), plid);
			SendClientMessage(playerid, -1, string222);

			format(string, sizeof(string), "{CCFF00}» EXP: %d/%d        {00FF00}» Grana na Mão: $%d", PlayerInfo[plid][_EXP], MAX_PLAYER_EXP, GetPlayerGrana(plid));
			SendClientMessage(playerid, -1, string);

			format(string, sizeof(string), "{CCFF00}» Level: %d/%d      {00FF00}» Saldo Bancário: $%d", PlayerInfo[plid][_Level], MAX_PLAYER_LEVEL, PlayerInfo[plid][_SaldoBancario]);
			SendClientMessage(playerid, -1, string);

			format(string, sizeof(string), "{CCFF00}» Estudo: %d/%d     {00FF00}» Casou Com: %s", PlayerInfo[plid][_Faculdade], MAX_PLAYER_ESTUDO, PlayerInfo[plid][_CasouCom]);
			SendClientMessage(playerid, -1, string);

			format(string, sizeof(string), "{CCFF00}» Profissão ID: %d  {00FF00}» Skin ID: %d", PlayerInfo[plid][_Profissao], PlayerInfo[plid][_Skin]);
			SendClientMessage(playerid, -1, string);

			format(string, sizeof(string), "{CCFF00}» Matou: %d         {00FF00}» Morreu: %d", matou[plid], morreu[plid]);
			SendClientMessage(playerid, -1, string);

			SendClientMessage(playerid, -1, string222);

			format(string, sizeof(string), "%s está vendo seu status. ( /status )", GetPlayerNameEx(playerid));
			SendClientMessage(plid, COLOR_GREEN, string);
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente!");
		}
		return 1;
	}

	if(strcmp(cmd, "/ttaxi", true) == 0 && IsPlayerConnected(playerid))
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Taxista || dini_Int(file, "aAdmin") == 1 || PlayerInfo[playerid][SCON] == true)
		{
			new plid;

			if(sscanf(cmdtext, "s[7]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "/ttaxi [id]");
				return 1;
			}
			if(GetPlayerVehicleID(playerid) == GetPlayerVehicleID(plid) && GetPlayerState(playerid) == PLAYER_STATE_DRIVER)
			{
				Taximetro[plid][0] = 1;
				Taximetro[plid][1] = playerid;

				GivePlayerGrana(playerid, 3);
				GivePlayerGrana(plid, 3);

				SendClientMessage(plid, COLOR_GREEN, "O taximêtro foi ligado, você pagará $3 cada metro.");
				SendClientMessage(playerid, COLOR_GREEN, "O taximêtro foi ligado, você receberá $3 cada metro.");
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "O passageiro não está dentro do seu taxi ou você está fora.");
			}
		}
		return 1;
	}

	if(strcmp("/setcar", cmd, true) == 0)
	{
		if(pAdmin[playerid] == 5)
		{
			new modelo, cor1, cor2;

			if(sscanf(cmdtext, "s[8]dD(-1)D(-1)", cmd, modelo, cor1, cor2))
			{
				SendClientMessage(playerid, tcadm, "Use: /setcar [modelo] [cor1] [cor2]");
				return 1;
			}
			if(modelo >= 400 && modelo <= 611)
			{
				if(IsPlayerInAnyVehicle(playerid))
				{
					if(modelo == 425 || modelo == 469 || modelo == 538 || modelo == 537 || modelo == 520 || modelo == 449 || modelo == 447 || modelo == 569 || modelo == 570 || modelo == 432)
					{
						SendClientMessage(playerid, tcadm, "Veículo proibido, tente outro! | ID's = 400-611");
						return 1;
					}

					new playerip[64], gString[256], File:temp,
						Float:X, Float:Y, Float:Z,
						Float:ang;

					GetPlayerPos(playerid, X, Y, Z);
					GetVehicleZAngle(GetPlayerVehicleID(playerid), ang);
					GetPlayerIp(playerid, playerip, sizeof(playerip));

					format(gString, sizeof(gString), "%d,%f,%f,%f,%f,%d,%d ; // Setado por %s (IP: %s)\r\n", modelo, X, Y, Z, ang, GetPlayerNameEx(playerid), playerip, cor1, cor2);
					temp = fopen("Conce/setados.txt", io_append);
					fwrite(temp, gString);
					fclose(temp);

					SetTimerEx("CriarVeiculo", 5000, false, "dffffdd", modelo, X, Y, Z, ang, cor1, cor2);
					printf("%s (%d) - Setou um carro!", GetPlayerNameEx(playerid), playerid);

					SendClientMessage(playerid, COLOR_GREEN, "Veículo setado com sucesso!");
					SendClientMessage(playerid, Amarelo, "O veículo aparecerá em instantes!");
				}
				else
				{
					SendClientMessage(playerid, tcadm, "Você não está em um veículo e assim não pode setar outro.");
				}
			}
			else
			{
				SendClientMessage(playerid, tcadm, "ID fora do normal! | ID's = 400-611");
			}
		}
		else
		{
			SendClientMessage(playerid, tcadm, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp(cmd, "/ad", true) == 0)
	{
		if(IsPlayerInRangeOfPoint(playerid, 50.0, 1541.9384765625, -1627.7314453125, 15.156204223633))
		{
			format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
			if(dini_Int(file, "Profissao") == Policial_M || dini_Int(file, "Profissao") == Policial_C || dini_Int(file, "Profissao") == Policial_F || dini_Int(file, "Profissao") == Delegado || dini_Int(file, "Profissao") == Bope || dini_Int(file, "Profissao") == Swat || dini_Int(file, "Profissao") == Narcoticos || dini_Int(file, "Profissao") == FBI || dini_Int(file, "Profissao") == Interpol || dini_Int(file, "Profissao") == LSPD || dini_Int(file, "aAdmin") == 1)
			{
				MoveDynamicObject(ObjectsFix[19], 1541.9780273438, -1639.4814453125, 15.156204223633, 3.0);
				SendClientMessage(playerid, 0xFFFFFFAA, "[PORTEIRO] O portão principal da DP foi aberto!");
			}
			else
			{
				SendClientMessage(playerid, Azul, "Você não é um militar!");
			}
		}
		else
		{
			SendClientMessage(playerid, 0xFF0000AA, "Você está muito longe do portão!");
		}
		return 1;
	}

	if(strcmp(cmd, "/ad2", true) == 0)
	{
		if(IsPlayerInRangeOfPoint(playerid, 50.0, 1588.0791015625, -1638.140625, 15.172611236572))
		{
			format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
			if(dini_Int(file, "Profissao") == Policial_M || dini_Int(file, "Profissao") == Policial_C || dini_Int(file, "Profissao") == Policial_F || dini_Int(file, "Profissao") == Delegado || dini_Int(file, "Profissao") == Bope || dini_Int(file, "Profissao") == Swat || dini_Int(file, "Profissao") == Narcoticos || dini_Int(file, "Profissao") == FBI || dini_Int(file, "Profissao") == Interpol || dini_Int(file, "Profissao") == LSPD || dini_Int(file, "aAdmin") == 1)
			{
				MoveDynamicObject(ObjectsFix[20], 1598.3291015625, -1638.1206054688, 15.172611236572, 3.0);
				SendClientMessage(playerid, 0xFFFFFFAA, "[PORTEIRO] O segundo portão da DP foi aberto!");
			}
			else
			{
				SendClientMessage(playerid, Azul, "Você não é um militar!");
			}
		}
		else
		{
			SendClientMessage(playerid, 0xFF0000AA, "Você está muito longe do portão!");
		}
		return 1;
	}

	if(strcmp(cmd, "/fd", true) == 0)
	{
		if(IsPlayerInRangeOfPoint(playerid, 50.0, 1541.9780273438, -1639.4814453125, 15.156204223633))
		{
			format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
			if(dini_Int(file, "Profissao") == Policial_M || dini_Int(file, "Profissao") == Policial_C || dini_Int(file, "Profissao") == Policial_F || dini_Int(file, "Profissao") == Delegado || dini_Int(file, "Profissao") == Bope || dini_Int(file, "Profissao") == Swat || dini_Int(file, "Profissao") == Narcoticos || dini_Int(file, "Profissao") == FBI || dini_Int(file, "Profissao") == Interpol || dini_Int(file, "Profissao") == LSPD || dini_Int(file, "aAdmin") == 1)
			{
				MoveDynamicObject(ObjectsFix[19], 1541.9384765625, -1627.7314453125, 15.156204223633, 3.0);
				SendClientMessage(playerid, 0xFFFFFFAA, "[PORTEIRO] O portão principal da DP foi fechado!");
			}
		}
		else
		{
			SendClientMessage(playerid, 0xFF0000AA, "Você está muito longe do portão!");
		}
		return 1;
	}

	if(strcmp(cmd, "/fd2", true) == 0)
	{
		if(IsPlayerInRangeOfPoint(playerid, 50.0, 1598.3291015625, -1638.1206054688, 15.172611236572))
		{
			format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
			if(dini_Int(file, "Profissao") == Policial_M || dini_Int(file, "Profissao") == Policial_C || dini_Int(file, "Profissao") == Policial_F || dini_Int(file, "Profissao") == Delegado || dini_Int(file, "Profissao") == Bope || dini_Int(file, "Profissao") == Swat || dini_Int(file, "Profissao") == Narcoticos || dini_Int(file, "Profissao") == FBI || dini_Int(file, "Profissao") == Interpol || dini_Int(file, "Profissao") == LSPD || dini_Int(file, "aAdmin") == 1)
			{
				MoveDynamicObject(ObjectsFix[20], 1588.0791015625, -1638.140625, 15.172611236572, 3.0);
				SendClientMessage(playerid, 0xFFFFFFAA, "[PORTEIRO] O segundo portão da DP foi fechado!");
			}
		}
		else
		{
			SendClientMessage(playerid, 0xFF0000AA, "Você está muito longe do portão!");
		}
		return 1;
	}

	if(strcmp(cmd, "/abrirbi", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Bibliotecario || dini_Int(file, "Profissao") == Professor || pAdmin[playerid] > 3)
		{
			Faculdade2 = 1;

			MoveDynamicObject(ObjectsFix[21], 1214.0789794922, -1842.5186767578, 20.415674209595, 4.0);
			MoveDynamicObject(ObjectsFix[22], 1269.8895263672, -1842.5379638672, 20.511180877686, 4.0);

			format(string, sizeof(string), "%s abriu a faculdade e a biblioteca.", GetPlayerNameEx(playerid));
			SendClientMessageToAll(0xCFCFCFAA, string);

			SendClientMessage(playerid, 0x607840AA, "Você abriu a faculdade e a biblioteca!");
			GameTextForAll("~w~Faculdade ~g~Aberta!", 6000, 1);
		}
		return 1;
	}

	if(strcmp(cmd, "/fecharbi", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Bibliotecario || dini_Int(file, "Profissao") == Professor || pAdmin[playerid] > 3)
		{
			Faculdade2 = 0;

			MoveDynamicObject(ObjectsFix[21], 1213.7843017578, -1842.4782714844, 15.156204223633, 4.0);
			MoveDynamicObject(ObjectsFix[22], 1270.2001953125, -1842.5798339844, 15.156204223633, 4.0);

			format(string, sizeof(string), "O(A) bibliotecário(a) %s fechou a faculdade e a biblioteca.", GetPlayerNameEx(playerid));
			SendClientMessageToAll(0xCFCFCFAA, string);

			SendClientMessage(playerid, 0x607840AA, "Você fechou a faculdade e a biblioteca!");
			GameTextForAll("~w~Faculdade ~r~Fechada!", 6000, 1);
		}
		return 1;
	}

	if(strcmp(cmd, "/previsao", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Meteorologista || dini_Int(file, "aAdmin") == 1 || PlayerInfo[playerid][SCON] == true)
		{
			new msg[256], texto[128];

			if(sscanf(cmdtext, "s[10]s[128]", cmd, texto))
			{
				SendClientMessage(playerid, Vermelho, "/previsao [texto]");
			}
			else
			{
				format(msg, sizeof(msg), "%s preve o clima: %s", GetPlayerNameEx(playerid), texto);
				SendClientMessageToAll(Verde, msg);
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	if(strcmp(cmd, "/descarregar", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Caminhoneiro)
		{
			if(IsPlayerInRangeOfPoint(playerid, 200.0, 2481.8823, -2089.1301, 13.5468) // Polo LS
				|| IsPlayerInRangeOfPoint(playerid, 200.0, -542.5219, -479.4108, 25.5178) // Polo SF
				|| IsPlayerInRangeOfPoint(playerid, 200.0, -1634.8437, 57.6602, 3.5546) // Polo SF
				|| IsPlayerInRangeOfPoint(playerid, 200.0, 1363.4182, 2259.7529, 11.2393) // Polo LV
				|| IsPlayerInRangeOfPoint(playerid, 200.0, 1041.8439, 2111.6079, 10.8203) // Polo LV
				|| IsPlayerInRangeOfPoint(playerid, 200.0, 2363.8598, 2756.6591, 10.8203)) // Polo LV
			{
				if(IsATruk(GetPlayerVehicleID(playerid)) && IsPlayerInAnyVehicle(playerid))
				{
					if(IsTrailerAttachedToVehicle(GetPlayerVehicleID(playerid)))
					{
						if(!IsPlayerInRangeOfPoint(playerid, 300.0, PlayerInfo[playerid][pPosX], PlayerInfo[playerid][pPosY], PlayerInfo[playerid][pPosZ]))
						{
							GivePlayerGrana(playerid, 500);
							PlayerInfo[playerid][pPoloArea] = 1;

							DetachTrailerFromVehicle(GetPlayerVehicleID(playerid));
							SendClientMessage(playerid, Verde, "Feito, você recebeu $500 pelo serviço.");
						}
						else
						{
							SendClientMessage(playerid, Vermelho, "Esta carga deve ser levada para o outro polo.");
						}
					}
					else
					{
						SendClientMessage(playerid, Vermelho, "Seu caminhão não está carregado.");
						SendClientMessage(playerid, Amarelo, "Engate-se em uma carga e leve-a até seu destino.");
					}
				}
				else
				{
					SendClientMessage(playerid, Vermelho, "Você não está em uma carreta.");
				}
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Você está muito longe do polo.");
				SendClientMessage(playerid, Vermelho, "Use /trabalhar para ir ao polo mais próximo.");
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não é um caminhoneiro credênciado.");
		}
		return 1;
	}

	if(strcmp(cmd, "/pescar", true) == 0)
	{
		if(IsPlayerInDynamicCP(playerid, CheckpointsFix[19]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[20]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[21]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[22]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[23]))
		{
			if(PescaInProgress[playerid] == 1)
			{
				SendClientMessage(playerid, Vermelho, "Você já lançou a vara aguarde até pegar um peixe.");
				return 1;
			}
			if(PescaInProgress[playerid] == 0)
			{
  				PescaInProgress[playerid] = 1;
				SetTimer("Pesca", 36000, false);

				SendClientMessage(playerid, 0x0080FFAA, "Você arremessou a vara. Espere até que o peixe morda a isca.");
				SendClientMessage(playerid, 0x0080FFAA, "Enquanto o peixe não morde, fique apreciando o ponto de pesca.");
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não está no ponto de pesca.");
		}
		return 1;
	}

	if(strcmp(cmd, "/venderpeixe", true) == 0)
	{
		if(IsPlayerInDynamicCP(playerid, CheckpointsFix[24]))
		{
			new msg[256], quantia;

			if(QtPescas[playerid] == 0)
			{
				SendClientMessage(playerid, Vermelho, "Você não pescou nada hoje, como quer vender?");
			}
			else
			{
				quantia = QtPescas[playerid]*50;
				GivePlayerGrana(playerid, quantia);
				QtPescas[playerid] = 0;

				format(msg, sizeof(msg), "Você vendeu %d peixe(s) por $50 cada um e recebeu $%d com a venda.", QtPescas[playerid], quantia);
				SendClientMessage(playerid, 0xFF0000AA, msg);
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não está no local de vender peixes.");
		}
		return 1;
	}

	if(strcmp(cmd, "/cortar", true) == 0)
	{
		if(IsPlayerInDynamicCP(playerid, CheckpointsFix[26]))
		{
			SetPlayerWantedLevel(playerid, 6);
			dini_IntSet(file, "Procurado", 1);
			Procurados[playerid] = 1;

			format(string, sizeof(string), "%s está cortando madeiras ilegais, e está sendo procurado(a)!", GetPlayerNameEx(playerid));
			SendClientMessageToAll(msgdm, string);

			if(cacando[playerid] == 1)
			{
				SendClientMessage(playerid, Vermelho, "Espere um pouco, você está cortando uma arvore com uma moto-serra nhennrnnnrnnnrnn.");
				return 1;
			}
			if(cacando[playerid] == 0)
			{
				cacando[playerid] = 1;
				SetTimer("cacas", 20000, false);

				SendClientMessage(playerid, 0x4E9C9CAA, "Você cortou uma grande arvore espere ela cair.");
				SendClientMessage(playerid, 0x4E9C9CAA, "Madeiiiiiiiiiiiiiiiraaaaa! aguarde alguns minutos até ela cair totalmente.");
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não está no check de cortar madeira.");
		}
		return 1;
	}

	if(strcmp(cmd, "/vendermadeira", true) == 0)
	{
		if(IsPlayerInDynamicCP(playerid, CheckpointsFix[27]))
		{
			new msg[256], quantia;

			if(Qtcacas[playerid] == 0)
			{
				SendClientMessage(playerid, Vermelho, "Você não cortou nenhuma madeira, como poderá vender?");
			}
			else
			{
				quantia = Qtcacas[playerid]*80;
				GivePlayerGrana(playerid, quantia);
				Qtcacas[playerid] = 0;

				format(msg, sizeof(msg), "Você vendeu %d madeira(s) ilegalmente por $80 cada uma e faturou $%d com a venda.", Qtcacas[playerid], quantia);
				SendClientMessage(playerid, COLOR_GREEN, msg);
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não está no check de vender a madeira ilegal.");
		}
		return 1;
	}

	if(strcmp(cmd, "/entregarpizza", true) == 0)
	{
		if(IsPlayerInDynamicCP(playerid, CheckpointsFix[18]))
		{
			if(Carregamento[playerid] == 0)
			{
				SendClientMessage(playerid, Vermelho, "Seu veículo não está carregado com uma pizza!");
			}
			else
			{
				Carregamento[playerid] = 0;
				GivePlayerGrana(playerid, 500);
				SendClientMessage(playerid, COLOR_GREEN, "Seu veículo foi descarregado e você ganhou $500.");
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não está no ponto de entregar pizza.");
		}
		return 1;
	}

	if(strcmp(cmd, "/pegarpizza", true) == 0)
	{
		if(GetVehicleModel(GetPlayerVehicleID(playerid)) == 448)
		{
			if(IsPlayerInDynamicCP(playerid, CheckpointsFix[12]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[13]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[14]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[15]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[16]) || IsPlayerInDynamicCP(playerid, CheckpointsFix[17]))
			{
				if(Carregamento[playerid] == 0)
				{
					Carregamento[playerid] = 1;
					ShowPlayerLocationFromGPS(playerid, "Entrega de Pizza", 2818.4600, -1620.4865, 11.0851);
					SendClientMessage(playerid, COLOR_GREEN, "Pizza pega!");
				}
				else
				{
					SendClientMessage(playerid, Vermelho, "Sua moto já está carregada!");
				}
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Você não está no check de pegar pizza!");
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não está em uma moto de entregar pizza.");
		}
		return 1;
	}

	if(strcmp(cmd, "/multar", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Policial_C || dini_Int(file, "Profissao") == Policial_F || dini_Int(file, "Profissao") == Delegado || dini_Int(file, "Profissao") == Bope || dini_Int(file, "Profissao") == Swat || dini_Int(file, "Profissao") == Narcoticos || dini_Int(file, "Profissao") == Policial_M || dini_Int(file, "Profissao") == FBI || dini_Int(file, "Profissao") == Interpol || dini_Int(file, "Profissao") == LSPD || dini_Int(file, "aAdmin") == 1 || PlayerInfo[playerid][SCON] == true)
		{
			new plid, quant;

			if(sscanf(cmdtext, "s[8]ud", cmd, plid, quant))
			{
				SendClientMessage(playerid, Vermelho, "/multar [id] [valor]");
				return 1;
			}
			if(quant > 1000 || quant <= 0)
			{
				SendClientMessage(playerid, Vermelho, "Você não está multando de forma justa.");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				if(infratores[plid] == 1)
				{
					format(file2, sizeof(file2), PASTA_CONTAS, GetPlayerNameEx(plid));

					infratores[plid] = 0;
					dini_IntSet(file2, "SaldoBancario", dini_Int(file2, "SaldoBancario")-quant);

					format(string, sizeof(string), "%s te deu uma multa no valor de $%d. O dinheiro foi retirado no banco.", GetPlayerNameEx(playerid), quant);
					SendClientMessage(plid, COLOR_GREEN, string);

					SendClientMessage(playerid, COLOR_GREEN, "Multa efetuada com sucesso!");
				}
				else
				{
					format(string, sizeof(string), "%s não é um(a) infrator(a) e não pode ser multado(a).", GetPlayerNameEx(plid));
					SendClientMessage(playerid, COLOR_RED, string);
				}
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "O(A) jogador(a) não está conectado!");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/convidarent", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Empregador || dini_Int(file, "Profissao") == Jornalista || dini_Int(file, "Profissao") == Reporter || dini_Int(file, "Profissao") == Ancora || dini_Int(file, "aAdmin") == 1 || PlayerInfo[playerid][SCON] == true)
		{
			new plid;

			if(sscanf(cmdtext, "s[13]u", cmd, plid))
			{
				SendClientMessage(playerid, COLOR_GREEN, "Use /convidarent [id]");
				return 1;
			}
			if(IsPlayerConnected(plid))
			{
				format(file2, sizeof(file2), PASTA_CONTAS, GetPlayerNameEx(plid));
				dini_IntSet(file2, "convitent", 1);

				format(string, sizeof(string), "%s (ID: %d) convidou você para participar de uma entrevista.", GetPlayerNameEx(playerid), playerid);
				SendClientMessage(plid, Blue, string);

				SendClientMessage(plid, Amarelo, "Para aceitar a entrevista digite /aceitar e para recusar a entrevista digite /recusar.");
				SendClientMessage(playerid, Verde, "Convite enviado!");
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Valor inválido, tente novamente!");
			}
		}
		return 1;
	}

	if(strcmp(cmd, "/sequestrar", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(PlayerInfo[playerid][SCON] == true || dini_Int(file, "Profissao") == Sequestrador || dini_Int(file, "Profissao") == Mafia)
		{
			new plid;

			if(sscanf(cmdtext, "s[12]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "/sequestrar [id]");
				return 1;
			}
			if(!IsPlayerConnected(plid))
			{
				SendClientMessage(playerid, Vermelho, "O(A) jogador(a) não está conectado");
				return 1 ;
			}
			if(jasequestro[playerid] == 1)
			{
				SendClientMessage(playerid, Vermelho, "Você já sequestrou alguem.");
				return 1;
			}
			if(sequestro[playerid] == 1)
			{
				SendClientMessage(playerid, Vermelho, "Você so pode sequestrar 1 por vez!");
				return 1;
			}
			if(plid == playerid)
			{
				SendClientMessage(playerid, Vermelho, "Você não pode fazer isto com si mesmo.");
				return 1 ;
			}
			if(IsPlayerInAnyVehicle(plid))
			{
				SendClientMessage(playerid, Vermelho, "O(A) jogador(a) que você quer sequestrar está em um veículo!");
			}
			else
			{
				if(IsPlayerInAnyVehicle(playerid))
				{
					if(GetDistanceBetweenPlayers(plid, playerid) < 10)
					{
						SetPlayerWantedLevel(playerid, 6);
						dini_IntSet(file, "Procurado", 1);

						Procurados[playerid] = 1;
						sequestro[playerid] = 1;
						jasequestro[playerid] = 1;

						TogglePlayerControllable(plid, 0);
						PutPlayerInVehicle(plid, GetPlayerVehicleID(playerid), 1);
						SetTimerEx("liberar", 300000, 1, "d", playerid);

						format(string, sizeof(string), "%s te sequestrou.", GetPlayerNameEx(playerid));
						SendClientMessage(plid, Blue, string);

						format(string, sizeof(string), "%s (ID: %d) sequestrou %s (ID: %d) e está sendo procurado(a).", GetPlayerNameEx(playerid), playerid, GetPlayerNameEx(plid), plid);
						SendClientMessageToAll(Amarelo, string);

						format(string, sizeof(string), "Tente pagar uma recompensa para salva-lo(a).");
						SendClientMessageToAll(Amarelo, string);

						SendClientMessage(playerid, COLOR_GREEN, "Player sequestrado!");
					}
					else
					{
						SendClientMessage(playerid, Vermelho, "Chegue mais perto para sequestrar!");
					}
				}
				else
				{
					SendClientMessage(playerid, Vermelho, "Você precisa estar em um veículo para sequestrar!");
				}
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Apenas gangster pode fazer isso!");
		}
		return 1;
	}

	if(strcmp(cmd, "/liberar", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(PlayerInfo[playerid][SCON] == true || dini_Int(file, "Profissao") == Sequestrador || dini_Int(file, "Profissao") == Mafia)
		{
			new plid;

			if(sscanf(cmdtext, "s[9]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "/liberar [id]");
				return 1;
			}
			if(!IsPlayerConnected(plid))
			{
				SendClientMessage(playerid, Vermelho, "O(A) jogador(a) não está conectado.");
				return 1;
			}
			if(sequestro[playerid] == 0)
			{
				SendClientMessage(playerid, Vermelho, "Você não sequestro ninguem para liberar!");
				return 1;
			}
			if(plid == playerid)
			{
				SendClientMessage(playerid, Vermelho, "Você não pode fazer isto com si mesmo.");
				return 1 ;
			}
			if(GetDistanceBetweenPlayers(plid, playerid) < 10)
			{
				sequestro[playerid] = 0;
				TogglePlayerControllable(plid, 1);
				RemovePlayerFromVehicle(plid);

				format(string, sizeof(string), "%s te liberou.", GetPlayerNameEx(playerid));
				SendClientMessage(plid, Blue, string);

				SendClientMessage(playerid, COLOR_GREEN, "Player liberado!");
			}
			else
			{
				SendClientMessage(playerid, Vermelho, "Chegue mais perto para liberar!");
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Apenas gangster pode fazer isso!");
		}
		return 1;
	}

	if(strcmp(cmd, "/amarrar", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Assaltante || dini_Int(file, "aAdmin") == 1 || PlayerInfo[playerid][SCON] == true)
		{
			new plid;

			if(sscanf(cmdtext, "s[9]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, " /amarrar [id]");
				return 1;
			}
			if(!IsPlayerConnected(plid))
			{
				SendClientMessage(playerid, Vermelho, "O(A) jogador(a) não está conectado.");
				return 1 ;
			}
			if(plid == playerid)
			{
				SendClientMessage(playerid, Vermelho, "Você não pode fazer isto com si mesmo.");
				return 1 ;
			}
			if(IsPlayerInAnyVehicle(plid) == 1 || IsPlayerInAnyVehicle(playerid) == 1)
			{
				SendClientMessage(playerid, Vermelho, "Alguem está dentro de um carro.");
			}
			else
			{
				if(GetDistanceBetweenPlayers(plid, playerid) < 10)
				{
					algemado[plid] = 1;
					TogglePlayerControllable(plid, 0);

					format(string, sizeof(string), "%s te amarrou.", GetPlayerNameEx(playerid));
					SendClientMessage(plid, Blue, string);

					SendClientMessage(playerid, COLOR_GREEN, "Amarrado com sucesso.");
				}
				else
				{
					SendClientMessage(playerid, Vermelho, "Chegue mais perto para amarrar.");
				}
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Apenas gangster pode fazer isso.");
		}
		return 1;
	}

	if(strcmp(cmd, "/desamarrar", true) == 0)
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Profissao") == Assaltante|| dini_Int(file, "aAdmin") == 1 || PlayerInfo[playerid][SCON] == true)
		{
			new plid;

			if(sscanf(cmdtext, "s[12]u", cmd, plid))
			{
				SendClientMessage(playerid, Vermelho, "/desamarrar [id]");
				return 1;
			}
			if(!IsPlayerConnected(plid))
			{
				SendClientMessage(playerid, Vermelho, "O(A) jogador(a) não está conectado.");
				return 1;
			}
			if(plid == playerid)
			{
				SendClientMessage(playerid, Vermelho, "Você não pode fazer isto com si mesmo.");
				return 1;
			}
			if(IsPlayerInAnyVehicle(plid) == 1 || IsPlayerInAnyVehicle(playerid) == 1)
			{
				SendClientMessage(playerid, Vermelho, "Alguem está dentro de um carro.");
			}
			else
			{
				if(GetDistanceBetweenPlayers(plid, playerid) < 10)
				{
					algemado[plid] = 0;
					TogglePlayerControllable(plid, 1);

					format(string, sizeof(string), "%s te desamarrou.", GetPlayerNameEx(playerid));
					SendClientMessage(plid, Blue, string);

					SendClientMessage(playerid, COLOR_GREEN, "Desamarrado com sucesso.");
				}
				else
				{
					SendClientMessage(playerid, Vermelho, "Chegue mais perto para desamarrar.");
				}
			}
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Apenas gangster pode fazer isso.");
		}
		return 1;
	}

	if(strcmp(cmd, "/trancar", true) == 0)
	{
		new Float:X, Float:Y, Float:Z;

		if(GetPlayerState(playerid) != PLAYER_STATE_DRIVER)
		{
			SendClientMessage(playerid, Vermelho, "Você só pode trancar o carro se estiver dirigindo.");
			return 1;
		}
		for(new i = 0; i < MAX_PLAYERS; i++)
		{
			if(IsPlayerConnected(i))
			{
				if(i != playerid)
				{
					SetVehicleParamsForPlayer(GetPlayerVehicleID(playerid), i, 0, 1);
				}
			}
		}
		GetPlayerPos(playerid, X, Y, Z);
		PlayerPlaySound(playerid, 1056, X, Y, Z);
		GameTextForPlayer(playerid, "~y~Veiculo ~r~Trancado", 5000, 6);
		return 1;
	}

	if(strcmp(cmd, "/destrancar", true) == 0)
	{
		new Float:X, Float:Y, Float:Z;

		if(GetPlayerState(playerid) != PLAYER_STATE_DRIVER)
		{
			SendClientMessage(playerid, Vermelho, "Você só pode destrancar o carro se estiver dirigindo.");
			return 1;
		}
		for(new i = 0; i < MAX_PLAYERS; i++)
		{
			if(IsPlayerConnected(i))
			{
				SetVehicleParamsForPlayer(GetPlayerVehicleID(playerid), i, 0, 0);
			}
		}
		GetPlayerPos(playerid, X, Y, Z);
		PlayerPlaySound(playerid, 1057, X, Y, Z);
		GameTextForPlayer(playerid, "~y~Veiculo ~g~Destrancado", 5000, 6);
		return 1;
	}

	if(strcmp(cmd, "/p", true) == 0)
	{
		new texto[128];

		if(sscanf(cmdtext, "s[3]s[128]", cmd, texto))
		{
			SendClientMessage(playerid, Vermelho, "/p [texto]");
			return 1;
		}
		ChatProximo(playerid, texto);
		return 1;
	}

	if(strcmp(cmd, "/infratores", true) == 0)
	{
		new count = 0, msg[1000];

		SendClientMessage(playerid, 0x008000AA, ".:: Infratores ::.");
		for(new i = 0; i < MAX_PLAYERS; i++)
		{
			if(IsPlayerConnected(i) && infratores[i])
			{
				format(msg, sizeof(msg), "%s (ID: %d)", GetPlayerNameEx(i), i);
				SendClientMessage(playerid, 0x0088CAAA, msg);
				count++;
			}
		}
		if(count == 0)
		{
			SendClientMessage(playerid, 0xFF0000AA, "Ninguém online cometeu infração!");
		}
		return 1;
	}

	if(strcmp(cmd, "/procurados", true) == 0)
	{
		new count = 0, msg[1000];

		SendClientMessage(playerid, 0x008000AA, ".:: Procurados ::.");
		for(new i = 0; i < MAX_PLAYERS; i++)
		{
			if(IsPlayerConnected(i) && Procurados[i])
			{
				format(msg, sizeof(msg), "%s (ID: %d)", GetPlayerNameEx(i), i);
				SendClientMessage(playerid, 0x0088CAAA, msg);
				count++;
			}
		}
		if(count == 0)
		{
			SendClientMessage(playerid, 0xFF0000AA, "Ninguém online está sendo procurado!");
		}
		return 1;
	}

	if(strcmp(cmd, "/presos", true) == 0)
	{
		new count = 0, msg[1000];

		SendClientMessage(playerid, 0x008000AA, ".:: Presos ::.");
		for(new i = 0; i < MAX_PLAYERS; i++)
		{
			if(IsPlayerConnected(i) && preso[i])
			{
				format(msg,sizeof(msg), "%s (ID: %d)", GetPlayerNameEx(i), i);
				SendClientMessage(playerid, 0x0088CAAA, msg);
				count++;
			}
		}
		if(count == 0)
		{
			SendClientMessage(playerid, 0xFF0000AA, "Ninguém online está preso!");
		}
		return 1;
	}

	if(!strcmp(cmd, "/minhacasa", true))
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Casa") == 1)
		{
			format(STRX, sizeof(STRX), "{0000FF}Ir (Ligar GPS até ela)\n{00FF00}Trancar/Destrancar\n{FFFF00}Convidar Morador\n{FF00DD}Expulsar Morador\n{FF0000}Vender Casa");
			ShowPlayerDialog(playerid, casaopt, DIALOG_STYLE_LIST, "Minha Casa", STRX, "OK", "Cancelar");
		}
		else
		{
			SendClientMessage(playerid, 0xFF0000AA, "Você não tem uma casa!");
		}
		return 1;
	}

	if(!strcmp(cmd, "/minhaprop", true))
	{
		format(file, sizeof(file), PASTA_CONTAS, GetPlayerNameEx(playerid));
		if(dini_Int(file, "Prop") == 1)
		{
			format(STRX, sizeof(STRX), "{0000FF}Ir/Entrar\n{00FF00}Sacar Renda\n{FFFF00}Mudar Nome\n{FF0000}Vender Prop");
			ShowPlayerDialog(playerid, propopt, DIALOG_STYLE_LIST, "Minha Propriedade", STRX, "OK", "Cancelar");
		}
		else
		{
			SendClientMessage(playerid, 0xFF0000AA, "Você não tem uma propriedade!");
		}
		return 1;
	}

	if(strcmp("/irpos", cmd, true) == 0)
	{
		if(pAdmin[playerid] == 5 || IsPlayerVIP(playerid))
		{
			new string2[256], Float:CorX, Float:CorY, Float:CorZ, xInterior;

			if(sscanf(cmdtext, "s[7]p<,>fffd", cmd, CorX, CorY, CorZ, xInterior))
			{
				SendClientMessage(playerid, Vermelho, "Use /irpos [x,y,z,i]");
				ShowPlayerDialog(playerid, playersimp, DIALOG_STYLE_MSGBOX, "Ajuda /irpos", "{00FFFF}Veja o exemplo de como proceder com uso deste comando:\n\n{FFFFFF}/irpos {FF0000}271.884979{FFFFFF},{0000FF}306.631988{FFFFFF},{00FF00}999.148437{FFFFFF},{FFFF00}2\n\n{00FFFF}Legenda:\n\n{FFFFFF}x = {FF0000}Posição X\n{FFFFFF}y = {0000FF}Posição Y\n{FFFFFF}z = {00FF00}Posição Z\n{FFFFFF}i = {FFFF00}Universo ID", "OK", "");
				return 1;
			}
			SetPlayerInterior(playerid, xInterior);
			SetPlayerPos(playerid, CorX, CorY, CorZ);
			SendClientMessage(playerid, -1, "Você foi até a posição:");
			format(string2, sizeof(string2), "{FF0000}%f{FFFFFF},{0000FF}%f{FFFFFF},{00FF00}%f{FFFFFF},{FFFF00}%d", CorX, CorY, CorZ, xInterior);
			SendClientMessage(playerid, -1, string2);
		}
		else
		{
			SendClientMessage(playerid, Vermelho, "Você não tem permissão.");
		}
		return 1;
	}

	new string2[256];
	format(string2, sizeof(string2), "Você digitou: %s - Comando inválido.", cmdtext);
	SendClientMessage(playerid, Vermelho, string2);
	return 1;
}

stock ShowPlayerLocationFromGPS(playerid, localname[], Float:PosX, Float:PosY, Float:PosZ)
{
	#if defined FGPSUser
	if(GetPVarInt(playerid, "YEAH") == 0)
	{
		new string2[128];

		SetPVarInt(playerid, "YEAH", 1);
		SetPVarFloat(playerid, "Spongebob", PosX);
		SetPVarFloat(playerid, "Mario", PosY);
		SetPVarFloat(playerid, "SpiderPig", PosZ);
		SetPVarString(playerid, "FAIL", localname);

		PlayerInfo[playerid][FGPS_RCP] = CreateDynamicRaceCP(2, PosX, PosY, PosZ, 0.0, 0.0, 0.0, 10.0, -1, -1, playerid, 100.0);
		PlayerInfo[playerid][FGPSObject] = CreateDynamicObject(1318, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, -1, -1, playerid, 50.0);

		format(string2, sizeof(string2), "{FFFFFF}Você esta indo para: {00FF33}%s{FFFFFF}.", localname);
		SendClientMessage(playerid, -1, string2);

		#if defined UseTd
		PlayerInfo[playerid][F_GPSTD] = CreatePlayerTextDraw(playerid, 37.000000, 290.000000, "~n~");
		PlayerTextDrawBackgroundColor(playerid, PlayerInfo[playerid][F_GPSTD], 255);
		PlayerTextDrawFont(playerid, PlayerInfo[playerid][F_GPSTD], 1);
		PlayerTextDrawLetterSize(playerid, PlayerInfo[playerid][F_GPSTD], 0.509999, 1.300000);
		PlayerTextDrawColor(playerid, PlayerInfo[playerid][F_GPSTD], -1);
		PlayerTextDrawSetOutline(playerid, PlayerInfo[playerid][F_GPSTD], 1);
		PlayerTextDrawSetProportional(playerid, PlayerInfo[playerid][F_GPSTD], 1);
		PlayerTextDrawShow(playerid, PlayerInfo[playerid][F_GPSTD]);
		#endif
	}
	#else
		#pragma unused playerid
		#pragma unused localname
		#pragma unused PosX
		#pragma unused PosY
		#pragma unused PosZ
	#endif

	return 1;
}