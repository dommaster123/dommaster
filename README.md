;~ #RequireAdmin
#Region ;**** Directives created by AutoIt3Wrapper_GUI ****
#AutoIt3Wrapper_Icon=TibiaInfo.ico
#AutoIt3Wrapper_Outfile=Black.Exe
#AutoIt3Wrapper_UseX64=n
#AutoIt3Wrapper_Res_Fileversion=0.0.0.93
#AutoIt3Wrapper_Res_Fileversion_AutoIncrement=y
#EndRegion ;**** Directives created by AutoIt3Wrapper_GUI ****
#include <IE.au3>
#include <Inet.au3>
#include <InetConstants.au3>
;************FOR GUI***********
#include <GUIConstantsEx.au3>
#include <GuiListBox.au3>
#include <WindowsConstants.au3>
#include <WinAPIFiles.au3>
;**********FOR BUTTON **********
#include <ButtonConstants.au3>
#include <StaticConstants.au3>
;**********FOR STRINGS **********
#include <StringConstants.au3>
#include <String.au3>
;**********FOR COMBOBOX **********
#include <StringConstants.au3>
#include <EditConstants.au3>
#include <GuiEdit.au3>
#include <GuiComboBox.au3>
;***********FOR LISTVIEW**********
#include <ListviewConstants.au3>
#include <GuiListView.au3>

;**********FOR FONTS **********
#include <FontConstants.au3>
;**********FOR PROCESS **********
#include <Process.au3>
#include <SendMessage.au3>

#include <Array.au3>

#include <Timers.au3>

#include <TabConstants.au3>
#include <ButtonConstants.au3>



#include <GDIPlus.au3>
#include <WinAPIDiag.au3>
#include <Misc.au3>
#include <ScreenCapture.au3>

#include <ColorConstants.au3>

#include <IsPressed_UDF.au3>

#include <_PixelGetColor.au3>
#include <Date.au3>


;===== MOUSE OPTIONS =========

Opt('MouseClickDelay', 0)
Opt('MouseClickDownDelay', 0)
Opt('MouseClickDragDelay', 0)
;===============================

;===== GUI EVENT OPTIONS ========
Opt("GUIOnEventMode", 1)
;===============================

;~ ConsoleWrite("######## PROJECT BLACK ##########")

;~ SET HOKTEYS IN TIBIA CLIENT !!!!!!!!!!!!!!!!!!!!
; [F1]  ==> high/low [ 100% ] odd/even [ 100% ] 1,2,3,4,5,6 [ 400% ] blackjack [ 100% ] [ Min 10k | Max 2kk ]
; [F2]  ==> Please message me in private chat and use always small letters !
; [F3]  ==> #s ~~WINNER WINNER CHICKEN DINNER !!! TRY YOUR LUCK NOW AND PLAY BLACKJACK !!!~~

Local Const $sFilePath = @ScriptDir & "\log.txt"
Local Const $sFilePathGameLog = @ScriptDir & "\GameLog.ini"
Local Const $sFilePathGameLogChat = @ScriptDir & "\GameLog.txt"


Global $ANTI_IDLE_TIME = (1000 * 60) * 10
Global $BLACK_STATUS = 0

;====== PAYOUT RATE =========
Global $HLPayout = 100
Global $NRPayout = 400
Global $XOPayout = 100
Global $BJPayout = 100


;===========================


;====== BLACK WINDOW ========
Global $BLACK_WINDOW_X
Global $BLACK_WINDOW_Y
Global $BLACK_WINDOW_WIDTH
Global $BLACK_WINDOW_HEIGHT
;===========================

Global $WIN_ANIMATION_POSITION[2]
Global $LOSE_ANIMATION_POSITION[2]

Global $VOODOOSKULL_POSITION[2]
Global $USE_VOODOOSKULL = True
Global $CHECK_LOCKER = True
Global $CHECK_COUNTER = True
Global $CHECK_PLAYER_SWITCH = True
Global $PLAYER_DETECTED = False
Global $PLAYER_BET = 0


Global $USE_VOODOOSKULL_DELAY = 1100
Global $LAST_USED_VOODOSKULL_TIME = TimerInit()


;======= SPECIAL FEATURES ==============
Global $JokeCollection[22] = ['Q: What did the dealer say to the deck of cards? A: "I can not deal with you anymore.', _
		'Q: What do craps dealers eat for dessert? A: Dice pudding.', _
		'Q: How is a casino like a good woman? A: Liquor in the front, poker in the back!', _
		'Q: What is the difference between prayer in church and prayer in a casino? A: In a casino, you really mean it!', _
		'Q: How can you tell if a poker player is bluffing? A: His chips are moving.', _
		'Q: When is the only time you split tens in BlackJack? A: When the table is full and your buddies need a seat.', _
		'Q: What kind of shark is always gambling? A: A CardShark.', _
		'Q: Why is not gambling allowed in Africa? A: Because of all the cheetahs.', _
		'Q: What does a gambling addict eat? A: Poker Chips and Salsa.', _
		'Q: How were Adam and Eve prevented from gambling? A: Their paradise (pair-o-dice) was taken away from them!', _
		'Q: What does a BlackJack player eat for dinner? A: Whatever his comp card allows him to.', _
		'Q: Whats the difference between poker players and politicans? A: Politicans tell the truth.', _
		'Q: Whats the difference between online poker and live poker? A: You can cry after a bad beat online and no one will laugh at you.', _
		'Q: What did a blonde from England bring a bag of french fries to a poker game? A: Someone told her to bring her own chips.', _
		'Q: What card game do lesbians play? A: Poke-her.', _
		'Q: What is the hardest thing about play mini baccarat? A: Telling your parents your gay!', _
		'Q: How do you get a professional poker player off your front porch? A: Pay him for the Pizza.', _
		'Last night I stayed up late playing poker with Tarot cards. I got a full house and four people died. ~Steven Wright', _
		'Poker is like sex - everyone thinks they are the best, but most people do not have a clue what they are doing. ~Dutch Boyd', _
		'If you are playing a poker game and look around the table and ca not tell who the sucker is, it is you. ~Paul Newman.', _
		' When a man with money meets a man with experience, the man with experience leaves with money and the man with money leaves with experience.', _
		'Trust everyone, but always cut the cards. ~Benny Binion']



;======== PLAYER CLICK POSITION =========
Global $PLAYER_NAME_RECTANGLE[4] = [100, 100, 100, 100]
Global $PLAYER_CLICK_POSITION[2]
Global $PLAYER_CHECKSUM
Global $CURRENT_CHARACTER_NAME = "[No Name]"
Global $NAME_WIDTH = 0
Global $STRING_BLOCK_SCAN_X
Global $STRING_BLOCK_SCAN_Y
Global $VISIT_PLAYER_LIST[][] = [["Dominik W"], ["Checksum"]]
Global $PLAYER_NAME_WIDTH
Global $CHAT_WINDOW_RECTANGLE[4]
Global $CHAT_WINDOW_CHECKSUM
Global $MSG_RECTANGLE[4]
;========================================
Global $MESSAGE_TO_STRING_RECTANGLE[4]
Global $CURRENT_MESSAGE_TO_STRING
Global $BLACK_START_TIME = TimerInit()

Global $ACTION_WINDOW_RECTANGLE[2]



Global $PLAYER_SWITCH_RECTANGLE[4]
Global $PLAYER_SWITCH_CHECKSUM
Global $CURRENT_PLAYER_SWITCH_CHECKSUM


Global $LAST_PLAYER_DETECT_DELAY = 50
Global $LAST_PLAYER_DETECT_TIME = TimerInit()

Global $looking_north = 0
Global $looking_east = 0
Global $looking_south = 0
Global $looking_west = 0



Global $LAST_COUNTER_CHECK_DELAY = 50
Global $LAST_COUNTER_CHECK_TIME = TimerInit()



Global $BOX17_RECTANGLE[4]
Global $BOX17_CHECKSUM

Global $DRAG_BOX16_POSITION[2]
Global $BOX16_CHECKSUM

Global $MOVE_OBJECT_OK_POSITION[2]

;========== A N T I    I D L E ===============
Global $BLACK_IDLE_TIME = TimerInit()
Global $ANTI_IDLE_TIME = (1000 * 60) * 10


Global $DEPOSIT_CONTAINER_POSITION

Global $AntiIdle = 1

Global $ITEM_DETECTED


Global $COUNTER_RECTANGLE[4]
Global $COUNTER_CHECKSUM

Global $LAST_COUNTER_CHECK_DELAY = 50
Global $LAST_COUNTER_CHECK_TIME = TimerInit()




Global $LOCKER_CHECKSUM

Global $LAST_LOCKER_CHECK_DELAY = 150
Global $LAST_LOCKER_CHECK_TIME = TimerInit()


Global $DEPOSIT_CONTAINER_POSITION[2]

Global $PLAYER_LOCKER_POSITION[2]
Global $AntiIdle = 1

Global $ArrayColor[177]
ColorArray()

;==== C O N T A I N E R =========

;========= Locker ==========
Global $LOCKER_RECTANGLE[4]
Global $LOCKER_DECORATION_RECTANGLE[4]
Global $DRAG_LOCKER_POSITION[4]

;========= Counter ==========
Global $COUNTER_RECTANGLE[4]
Global $DRAG_COUNTER_POSITION[4]

;========= Box 4 (Effect Box) ==========
Global $BOX4_RECTANGLE[4]
;========= Box 15 (Platinum Coin Container) ==========
Global $BOX15_RECTANGLE[4]
;========= Box 16 (Crystal Coin Container) ==========
Global $BOX16_RECTANGLE[4]
;========= Box 17 (Temp Container) ==========
Global $BOX17_RECTANGLE[4]

Global $LAST_CHECK_CHAT = TimerInit()
Global $CHECK_CHAT_DELAY = 1000

Global $TIBIA_ACTION_WINDOW_CENTRE_X
Global $TIBIA_ACTION_WINDOW_CENTRE_Y
Global $TIBIA_ACTION_WINDOW_RECTANGLE[4]
Global $TILE_WIDTH
Global $TILE_HEIGHT
Global $CHARACTER_TILE_RECTANGLE[4]
Global $PLAYER_TILE_RECTANGLE[4]
Global $CHECKSUMS[1]

Global $PLAYER_SWITCH_CHECK_TIME = TimerInit()
Global $PLAYER_SWITCH_CHECK_DELAY = 0

Global $PLAYER_LIST[0][6]

Global $SCAM_LEVEL = 10

Global $BOX17_NUMBER_RECTANGLE[4]


Global $BLACK_SOUTH = 0
Global $BLACK_NORTH = 0
Global $BLACK_EAST = 0
Global $BLACK_WEST = 0

Global $CONTAINERS_READY = False


Global $LOCKER_CLICK_POSITION[2]
Global $DRAG_BOX15_POSITION[2]

Global $OPEN_IN_NEW_WINDOW_CLICK_POSITION[2]
Global $BROWSE_FIELD_LOCKER_CLICK_POSITION[2]
Global $LOCKER_SLOT1_CLICK_POSITION[2]

Global $CURRENT_BLACK_BALANCE = 0

Global $CLIENT_CHARACTER_NAME

Global $CASINO_PROFIT = 1

Global $LAST_ANNOUNCEMENT_TIME = TimerInit()
Global $PLAYER_MSG_STRING_BLOCK_SCAN_X
Global $PLAYER_MSG_STRING_BLOCK_SCAN_Y
Global $CURRENT_PLAYER_MSG

Global Const $ODD_NUMBER_LIST = [1, 3, 5]
Global Const $EVEN_NUMBER_LIST = [2, 4, 6]
Global Const $HIGH_NUMBER_LIST = [4, 5, 6]
Global Const $LOW_NUMBER_LIST = [1, 2, 3]
Global Const $FULL_NUMBER_LIST = [1, 2, 3, 4, 5, 6]

Global $STORE_RECTANGLE[4]

Global $OVER_BALANCE = 300

Global $BLACKJACK_ACTIVE_GAME = 0
Global $bj_host_total
Global $bj_customer_total

Global $BLACKJACK_SCAM_ACTIVE = False

Global $MESSAGE_DELAY = 700 ; in Sekunden


HotKeySet("^{DEL}", "StopBlack")


GUI_Main()



While 1

	If $BLACK_STATUS == 1 Then
		AntiIdle()
		CheckOverBalance()
		UpdateProfitHourHUD()
		Announcement()
		SpecialEventAnnouncement()
		RandomJokes()
		If TibiaWindowExists() And TibiaWindowIsFocused() And CharacterLogedIn() Then
			UseVoodooSkull()
			DetectPlayer()
			EatSpecialItems()
			CheckCounter()
			CheckLocker()
			DetectChat()
		ElseIf TibiaWindowExists() And CharacterLogedIn() And Not TibiaWindowIsFocused() Then
			ActivateTibiaWindow()
		ElseIf Not CharacterLogedIn() Then
			StopBlack()

		EndIf
	EndIf

	Sleep(10)

;~ If TibiaWindowExists() And TibiaWindowIsFocused() And CharacterLogedIn() Then
;~ 	CloseBlack()
;~ EndIf

WEnd

Func GUI_Main()
	Local $FontColor = "0xfefefe", $TextBkColor = "0x363636"

	Global $GUI_Main = GUICreate("Black", 220, 220, -1, -1, -1, $WS_EX_TOPMOST)
	GUISetOnEvent($GUI_EVENT_CLOSE, "GUI_Close")

	Global $label_status = GUICtrlCreateLabel("Status:", 25, 10, 40)
	GUICtrlSetColor(-1, $FontColor)
	Global $status = GUICtrlCreateLabel("offline", 70, 10, 160)
	GUICtrlSetColor(-1, $FontColor)


	Global $label_host_name = GUICtrlCreateLabel("Host:", 25, 110, 45)
	GUICtrlSetColor(-1, $FontColor)
	Global $host_name = GUICtrlCreateLabel("[No Name]", 100, 110, 160, 25)
	GUICtrlSetColor(-1, $FontColor)

	Global $label_host_balance = GUICtrlCreateLabel("Balance:", 25, 130, 45)
	GUICtrlSetColor(-1, $FontColor)
	Global $host_balance = GUICtrlCreateLabel($CURRENT_BLACK_BALANCE & " cc", 100, 130, 160)
	GUICtrlSetColor(-1, $FontColor)

	Global $label_player = GUICtrlCreateLabel("Player:", 25, 150, 45)
	GUICtrlSetColor(-1, $FontColor)
	Global $black_player = GUICtrlCreateLabel($CURRENT_CHARACTER_NAME, 100, 150, 160)
	GUICtrlSetColor(-1, $FontColor)

	Global $label_black_player_profit = GUICtrlCreateLabel("Profit:", 25, 170, 45)
	GUICtrlSetColor(-1, $FontColor)
	Global $player_profit = GUICtrlCreateLabel(0 & " cc", 100, 170, 160)
	GUICtrlSetColor(-1, $FontColor)

	Global $label_host_profit_per_hour = GUICtrlCreateLabel("Profit/Hour:", 25, 190, 55)
	GUICtrlSetColor(-1, $FontColor)
	Global $host_profit_per_hour = GUICtrlCreateLabel(0 & " cc", 100, 190, 160)
	GUICtrlSetColor(-1, $FontColor)




	;==================== TOOLS CHECKBOX & BUTTON ===============================
	GUICtrlCreateButton("Scan", 25, 30, 80, 20)
	GUICtrlSetOnEvent(-1, "PositionScan")
	GUICtrlSetColor(-1, $FontColor)
	GUICtrlSetBkColor(-1, 0x696969)


	GUICtrlCreateButton("Info", 25, 50, 80, 20)
	GUICtrlSetOnEvent(-1, "GUI_Info")
	GUICtrlSetColor(-1, $FontColor)
	GUICtrlSetBkColor(-1, 0x696969)


	Global $input_Scan_info = GUICtrlCreateInput("", 25, 30)
	GUICtrlSetState($input_Scan_info, $GUI_HIDE)






	GUICtrlCreateLabel("Start", 140, 30)
	GUICtrlSetColor(-1, $FontColor)
	Global $chkbox_Start = GUICtrlCreateCheckbox("", 120, 30, 15, 15)
	GUICtrlSetOnEvent($chkbox_Start, "OnClick_chkbox_Start")
	GUICtrlSetColor(-1, $FontColor)

	GUICtrlCreateLabel("Turn Off DWM", 140, 45)
	GUICtrlSetColor(-1, $FontColor)
	Global $chkbox_DWM = GUICtrlCreateCheckbox("", 120, 45, 15, 15)
	GUICtrlSetOnEvent($chkbox_DWM, "OnClick_chkbox_DWM")
	GUICtrlSetColor(-1, $FontColor)

	GUIStartGroup()
	GUICtrlCreateLabel("North Spot", 45, 70)
	GUICtrlSetColor(-1, $FontColor)
	Global $radiobox_north = GUICtrlCreateRadio("", 25, 70, 15, 15)
	GUICtrlSetOnEvent($radiobox_north, "OnClick_radiobox_north")
	GUICtrlSetColor(-1, $FontColor)

	GUICtrlCreateLabel("South Spot", 45, 90)
	GUICtrlSetColor(-1, $FontColor)
	Global $radiobox_south = GUICtrlCreateRadio("", 25, 90, 15, 15)
	GUICtrlSetOnEvent($radiobox_south, "OnClick_radiobox_south")
	GUICtrlSetColor(-1, $FontColor)

	GUIStartGroup()
	GUICtrlCreateLabel("West Spot", 140, 70)
	GUICtrlSetColor(-1, $FontColor)
	Global $radiobox_west = GUICtrlCreateRadio("", 120, 70, 15, 15)
	GUICtrlSetOnEvent($radiobox_west, "OnClick_radiobox_west")
	GUICtrlSetColor(-1, $FontColor)

	GUICtrlCreateLabel("East Spot", 140, 90)
	GUICtrlSetColor(-1, $FontColor)
	Global $radiobox_east = GUICtrlCreateRadio("", 120, 90, 15, 15)
	GUICtrlSetOnEvent($radiobox_east, "OnClick_radiobox_east")
	GUICtrlSetColor(-1, $FontColor)



	GUISetBkColor(0x4c4c4c, $GUI_Main)
	GUISetState(@SW_SHOW, $GUI_Main)
EndFunc   ;==>GUI_Main
Func GUI_Close()
;~ 	DllCall("dwmapi.dll", "hwnd", "DwmEnableComposition", "uint", 1)
	GUIDelete($GUI_Main)
	Exit
EndFunc   ;==>GUI_Close
Func GetHostName()
	Local $tempTitle = WinGetTitle(WinGetHandle("[CLASS:Qt5QWindowIcon]"))
	Local $split = StringSplit($tempTitle, " - ", 1)

	Return $split[2]
EndFunc   ;==>GetHostName

Func GUI_Info()

	GUIDelete($GUI_Main)

	Local $FontColor = "0xfefefe", $TextBkColor = "0x363636"

	Global $GUI_Info = GUICreate("nobelona", 220, 220, -1, -1, -1, $WS_EX_TOPMOST)
	GUISetOnEvent($GUI_EVENT_CLOSE, "GUI_CloseInfo")


	Global $label_status = GUICtrlCreateLabel("Status:", 25, 10, 40)
	GUICtrlSetColor(-1, $FontColor)
	Global $status = GUICtrlCreateLabel("offline", 70, 10, 160)
	GUICtrlSetColor(-1, $FontColor)


	Global $label_host_name = GUICtrlCreateLabel("Host:", 25, 110, 45)
	GUICtrlSetColor(-1, $FontColor)
	Global $host_name = GUICtrlCreateLabel("[No Name]", 100, 110, 160, 25)
	GUICtrlSetColor(-1, $FontColor)

	Global $label_casino_balance = GUICtrlCreateLabel("Balance:", 25, 130, 45)
	GUICtrlSetColor(-1, $FontColor)
	Global $host_balance = GUICtrlCreateLabel($CURRENT_BLACK_BALANCE & " cc", 100, 130, 160)
	GUICtrlSetColor(-1, $FontColor)

	Global $label_casino_player = GUICtrlCreateLabel("Player:", 25, 150, 45)
	GUICtrlSetColor(-1, $FontColor)
	Global $black_player = GUICtrlCreateLabel($CURRENT_CHARACTER_NAME, 100, 150, 160)
	GUICtrlSetColor(-1, $FontColor)

	Global $label_black_player_profit = GUICtrlCreateLabel("Profit:", 25, 170, 45)
	GUICtrlSetColor(-1, $FontColor)
	Global $player_profit = GUICtrlCreateLabel(0 & " cc", 100, 170, 160)
	GUICtrlSetColor(-1, $FontColor)

	Global $label_host_profit_per_hour = GUICtrlCreateLabel("Profit/Hour:", 25, 190, 55)
	GUICtrlSetColor(-1, $FontColor)
	Global $host_profit_per_hour = GUICtrlCreateLabel(0 & " cc", 100, 190, 160)



	GUISetBkColor(0x4c4c4c, $GUI_Info)
	GUISetState(@SW_SHOW, $GUI_Info)
EndFunc   ;==>GUI_Info
Func GUI_CloseInfo()
	GUIDelete($GUI_Info)
	GUI_Main()
EndFunc   ;==>GUI_CloseInfo

;====== BUTTON FUNCTIONS ==========
Func PositionScan()

	If ($BLACK_NORTH <> True And $BLACK_SOUTH <> True) Or ($BLACK_EAST <> True And $BLACK_WEST <> True) Then
		_GUICtrlEdit_ShowBalloonTip($input_Scan_info, "", "Please select below your locker position first!", $TTI_INFO)
	Else
		ClearLogFile()

		;==> Tibia Client
		SaveCurrentTibiaWindowPos(1)
		SaveCurrentChatWindowCheckSum(1)

		;==> Store Button
		GetStoreRectangle()

		;==> Chat Window
		GetChatWindowRectangle(1)

		;==> Action Window
		GetTibiaActionWindowRectangle(1)
		GetCharacterTileRectangle(1)
		GetPlayerTileRectangle(1)
		SaveCurrentPlayerCheckSum(1)
		GetPlayerClickPosition(1)
		GetPlayerLockerDragPosition(1)
		GetLockerClickToOpenPosition()


		;==> Locker
		If Not GetLockerPosition(0) Then
			OpenContainers()
			PositionScan()
			Return
		EndIf
		SaveCurrentLockerCheckSum(1)
		GetLockerDecorationRectangle(1)
		GetLockerDragPosition(1)


		;==> Counter
		GetBrowseFieldCounterPosition(1)
		SaveCurrentCounterCheckSum(1)
		GetCounterDragPosition(1)


		;==> Box17
		GetBox17Position()
		SaveCurrentBox17CheckSum(1)
		GetBox17NumberRectangle()
		GetTempContainerDepositItemsPosition()

		;==> Box16
		GetBox16Position()
		GetBox16DragPosition()

		;==> Box15
		GetBox15Position()
		GetBox15DragPosition()

		;==> Box4
		GetBox4Position()
		GetLockerEffectItemPosition()
		GetWinEffectItemPosition()
;~ GetLoseEffectItemPosition()



		GetMoveObjectsWindowOkPosition()

		If $BLACK_WINDOW_HEIGHT <> 0 And $BLACK_WINDOW_WIDTH <> 0 And _
				$CHAT_WINDOW_RECTANGLE[0] <> 0 And _
				$TIBIA_ACTION_WINDOW_RECTANGLE[2] <> 0 And $TIBIA_ACTION_WINDOW_RECTANGLE[3] <> 0 And _
				$MOVE_OBJECT_OK_POSITION[0] <> 0 And _
				$COUNTER_RECTANGLE[0] <> 0 And _
				$DRAG_COUNTER_POSITION[0] <> 0 And _
				$LOCKER_RECTANGLE[0] <> 0 And _
				$LOCKER_DECORATION_RECTANGLE[0] <> 0 And _
				$BOX17_RECTANGLE[0] <> 0 And _
				$DEPOSIT_CONTAINER_POSITION[0] <> 0 And _
				$BOX16_RECTANGLE[0] <> 0 And _
				$DRAG_BOX16_POSITION[0] <> 0 And _
				$BOX15_RECTANGLE[0] <> 0 And _
				$BOX4_RECTANGLE[0] <> 0 And _
				$VOODOOSKULL_POSITION[0] <> 0 And _
				$WIN_ANIMATION_POSITION[0] <> 0 Then

			$CONTAINERS_READY = True
			GUICtrlSetData($status, "online")

			SetHostName()
			SetCasinoBalance()
		Else
			GUICtrlSetData($status, "offline")
			$CONTAINERS_READY = False
		EndIf

	EndIf
EndFunc   ;==>PositionScan
;==================================
Func CloseBlack()

	Local $tempServerSave_minutes

	If @HOUR < 10 Then
		$tempServerSave_minutes = _DateDiff("n", _NowCalc(), @YEAR & "/" & @MON & "/" & @MDAY & " 10:00:00")
	ElseIf @HOUR >= 10 Then
		$tempServerSave_minutes = _DateDiff("n", _NowCalc(), @YEAR & "/" & @MON & "/" & @MDAY + 1 & " 10:00:00")
	EndIf

	ConsoleWrite(@CRLF & "TIME TO SS: " & $tempServerSave_minutes)

	If $tempServerSave_minutes <= 5 Then

		CasinoPickUpDecoration()
		StopBlack()
		RunWait('taskkill /F /IM "client.exe"', "", @SW_HIDE)
		Shutdown(1)
	EndIf

EndFunc   ;==>CloseBlack

Func TibiaPing()
	$tempPing = Ping("107.154.50.250")
	Return $tempPing
EndFunc   ;==>TibiaPing
Func OpenContainers()

;~ 	GetLockerClickToOpenPosition()
;~ 	OpenLocker()
	GetLockerClickToOpenPosition()
	RightClickLocker()
	GetBrowseFieldLockerRectangle()
	OpenBrowseFieldLocker()
	GetLockerPosition(1)

	GetLockerSlot1ClickToOpenPosition()
	RightClickLockerSlot1()
	GetOpenInNewWindowLockerRectangle()
	OpenInNewWindowLockerSlot1()




EndFunc   ;==>OpenContainers
Func CasinoPickUpDecoration()

	If $BLACK_STATUS == 1 Then
		While Not CheckIfEmptySlot($LOCKER_RECTANGLE[0] + 37, $LOCKER_RECTANGLE[1])
			ConsoleWrite(@CRLF & "PICKUP LOCKER DECORATION")
			MouseClickDrag($MOUSE_CLICK_LEFT, $DRAG_LOCKER_POSITION[0] - 37, $DRAG_LOCKER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
			MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
		WEnd
	EndIf

EndFunc   ;==>CasinoPickUpDecoration
Func CheckOverBalance()

	ConsoleWrite(@CRLF & "Start CheckOverBalance()")

	Local $tempBalance = 0

	If CheckBalance() >= $OVER_BALANCE Then
		$tempBalance = CheckBalance()

		While $tempBalance == CheckBalance()
			ConsoleWrite(@CRLF & $tempBalance & "cc was scanned before <=> current Balance: " & CheckBalance())
			Sleep(250)
			Send("{CTRLDOWN}")
			MouseClickDrag($MOUSE_CLICK_LEFT, $DRAG_BOX16_POSITION[0], $DRAG_BOX16_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 5)
			Send("{CTRLUP}")
			Sleep(250)
;~ 			MouseClick($MOUSE_CLICK_LEFT, $MOVE_OBJECT_OK_POSITION[0], $MOVE_OBJECT_OK_POSITION[1], 1, 0)
			Send("{ENTER}")
			Sleep(1000)
		WEnd
		SetCasinoBalance()
	EndIf

EndFunc   ;==>CheckOverBalance

Func SetHostName()
	$CLIENT_CHARACTER_NAME = GetHostName()
	GUICtrlSetData($host_name, $CLIENT_CHARACTER_NAME)
EndFunc   ;==>SetHostName
Func SetCasinoBalance()
	$CURRENT_BLACK_BALANCE = CheckBalance()
	GUICtrlSetData($host_balance, $CURRENT_BLACK_BALANCE & " cc")
EndFunc   ;==>SetCasinoBalance

Func RightClickLocker()
	Send("{CTRLDOWN}")
	MouseClick($MOUSE_CLICK_RIGHT, $LOCKER_CLICK_POSITION[0], $LOCKER_CLICK_POSITION[1], 1, 0)
	Send("{CTRLUP}")
EndFunc   ;==>RightClickLocker
Func GetBrowseFieldLockerRectangle()

	Local $TopLeft_X
	Local $TopLeft_Y

	Local $time = TimerInit()


	$aCoord = PixelSearch($LOCKER_CLICK_POSITION[0], $LOCKER_CLICK_POSITION[1], $LOCKER_CLICK_POSITION[0] + 1, $LOCKER_CLICK_POSITION[1] + 350, 0x171717)
	While $aCoord == 0
		$aCoord = PixelSearch($LOCKER_CLICK_POSITION[0], $LOCKER_CLICK_POSITION[1], $LOCKER_CLICK_POSITION[0] + 1, $LOCKER_CLICK_POSITION[1] + 350, 0x171717)
		Sleep(10)
	WEnd

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)


	Local $posY = $LOCKER_CLICK_POSITION[1] + 40
	Local $BrowseFieldFound = False
	$col1 = PixelSearch($LOCKER_CLICK_POSITION[0] + 7, $LOCKER_CLICK_POSITION[1] + 40, $LOCKER_CLICK_POSITION[0] + 7, $LOCKER_CLICK_POSITION[1] + 315, 0x000000)

	While Not $BrowseFieldFound And $posY < $LOCKER_CLICK_POSITION[1] + 315
		If $col1 <> 0 And _
				_PixelGetColor_GetPixel($vDC, $col1[0] + 1, $col1[1], $hDll) == "F7F7F7" And _
				_PixelGetColor_GetPixel($vDC, $col1[0] + 2, $col1[1], $hDll) == "F7F7F7" And _
				_PixelGetColor_GetPixel($vDC, $col1[0] + 3, $col1[1], $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $col1[0] + 4, $col1[1], $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $col1[0] + 5, $col1[1], $hDll) == "F7F7F7" And _
				_PixelGetColor_GetPixel($vDC, $col1[0] + 6, $col1[1], $hDll) == "F7F7F7" Then
			$BrowseFieldFound = True
			ConsoleWrite(@CRLF & "[Browse Field] String found: " & $col1[0] & "x" & $col1[1])
			ExitLoop
		EndIf
		If $col1 <> 0 Then
			$posY = $col1[1] + 1
		Else
			$posY = $posY + 1
		EndIf
		$col1 = PixelSearch($LOCKER_CLICK_POSITION[0], $posY, $LOCKER_CLICK_POSITION[0] + 7, $LOCKER_CLICK_POSITION[1] + 315, 0x000000)
	WEnd

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $col1 == 0 Then
		Return False
	EndIf

	$TopLeft_X = $col1[0] + 30
	$TopLeft_Y = $col1[1] + 2

	Local $tempArray = [$TopLeft_X, $TopLeft_Y]

	$BROWSE_FIELD_LOCKER_CLICK_POSITION = $tempArray

	ConsoleWrite(@CRLF & "$BROWSE_FIELD_CLICK_POSITION: " & $BROWSE_FIELD_LOCKER_CLICK_POSITION[0] & "x" & $BROWSE_FIELD_LOCKER_CLICK_POSITION[1])

EndFunc   ;==>GetBrowseFieldLockerRectangle
Func OpenBrowseFieldLocker()
	MouseClick($MOUSE_CLICK_LEFT, $BROWSE_FIELD_LOCKER_CLICK_POSITION[0], $BROWSE_FIELD_LOCKER_CLICK_POSITION[1], 1, 0)
EndFunc   ;==>OpenBrowseFieldLocker

Func GetLockerSlot1ClickToOpenPosition()

	$LOCKER_SLOT1_CLICK_POSITION[0] = $LOCKER_RECTANGLE[0] + 15
	$LOCKER_SLOT1_CLICK_POSITION[1] = $LOCKER_RECTANGLE[1] + 15

	ConsoleWrite(@CRLF & "$LOCKER_SLOT1_CLICK_POSITION: " & $LOCKER_SLOT1_CLICK_POSITION[0] & "x" & $LOCKER_SLOT1_CLICK_POSITION[1])
EndFunc   ;==>GetLockerSlot1ClickToOpenPosition
Func RightClickLockerSlot1()
	Send("{CTRLDOWN}")
	MouseClick($MOUSE_CLICK_RIGHT, $LOCKER_SLOT1_CLICK_POSITION[0], $LOCKER_SLOT1_CLICK_POSITION[0], 1, 0)
	Send("{CTRLUP}")
EndFunc   ;==>RightClickLockerSlot1
Func GetOpenInNewWindowLockerRectangle()

	Local $TopLeft_X
	Local $TopLeft_Y

	Local $time = TimerInit()


	$aCoord = PixelSearch($LOCKER_SLOT1_CLICK_POSITION[0], $LOCKER_SLOT1_CLICK_POSITION[1], $LOCKER_SLOT1_CLICK_POSITION[0] + 1, $LOCKER_SLOT1_CLICK_POSITION[1] + 350, 0x171717)
	While $aCoord == 0
		$aCoord = PixelSearch($LOCKER_SLOT1_CLICK_POSITION[0], $LOCKER_SLOT1_CLICK_POSITION[1], $LOCKER_SLOT1_CLICK_POSITION[0] + 1, $LOCKER_SLOT1_CLICK_POSITION[1] + 350, 0x171717)
		Sleep(10)
	WEnd

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)


	Local $posY = $LOCKER_SLOT1_CLICK_POSITION[1] + 40
	Local $OpenInNewWindowFound = False
	$col1 = PixelSearch($LOCKER_SLOT1_CLICK_POSITION[0] + 7, $LOCKER_SLOT1_CLICK_POSITION[1] + 40, $LOCKER_SLOT1_CLICK_POSITION[0] + 7, $LOCKER_SLOT1_CLICK_POSITION[1] + 315, 0x000000)

	While Not $OpenInNewWindowFound And $posY < $LOCKER_SLOT1_CLICK_POSITION[1] + 315
		If $col1 <> 0 And _
				_PixelGetColor_GetPixel($vDC, $col1[0] + 1, $col1[1], $hDll) == "F7F7F7" And _
				_PixelGetColor_GetPixel($vDC, $col1[0] + 2, $col1[1], $hDll) == "F7F7F7" And _
				_PixelGetColor_GetPixel($vDC, $col1[0] + 3, $col1[1], $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $col1[0] + 4, $col1[1], $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $col1[0] + 5, $col1[1], $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $col1[0] + 6, $col1[1], $hDll) == "F7F7F7" And _
				_PixelGetColor_GetPixel($vDC, $col1[0] + 7, $col1[1], $hDll) == "F7F7F7" Then
			$OpenInNewWindowFound = True
			ConsoleWrite(@CRLF & "[$OpenInNewWindowFound] String found: " & $col1[0] & "x" & $col1[1])
			ExitLoop
		EndIf
		If $col1 <> 0 Then
			$posY = $col1[1] + 1
		Else
			$posY = $posY + 1
		EndIf
		$col1 = PixelSearch($LOCKER_SLOT1_CLICK_POSITION[0], $posY, $LOCKER_SLOT1_CLICK_POSITION[0] + 7, $LOCKER_SLOT1_CLICK_POSITION[1] + 315, 0x000000)
	WEnd

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $col1 == 0 Then
		Return False
	EndIf

	$TopLeft_X = $col1[0] + 30
	$TopLeft_Y = $col1[1] + 2

	Local $tempArray = [$TopLeft_X, $TopLeft_Y]

	$OPEN_IN_NEW_WINDOW_CLICK_POSITION = $tempArray

	ConsoleWrite(@CRLF & "$LOCKER_SLOT1_CLICK_POSITION: " & $OPEN_IN_NEW_WINDOW_CLICK_POSITION[0] & "x" & $OPEN_IN_NEW_WINDOW_CLICK_POSITION[1])

EndFunc   ;==>GetOpenInNewWindowLockerRectangle
Func OpenInNewWindowLockerSlot1()
	MouseClick($MOUSE_CLICK_LEFT, $OPEN_IN_NEW_WINDOW_CLICK_POSITION[0], $OPEN_IN_NEW_WINDOW_CLICK_POSITION[1], 1, 0)
EndFunc   ;==>OpenInNewWindowLockerSlot1

Func RightClickPlayer()

	Send("{CTRLDOWN}")
	MouseClick($MOUSE_CLICK_RIGHT, $PLAYER_CLICK_POSITION[0], $PLAYER_CLICK_POSITION[1], 1, 0)
;~ 	ControlClick("[CLASS:Qt5QWindowIcon]", "", "", "right", 1, $PLAYER_CLICK_POSITION[0]-8, $PLAYER_CLICK_POSITION[1]-8)
	Send("{CTRLUP}")
	Sleep(500)
EndFunc   ;==>RightClickPlayer
Func GetCharacterNameRectangle()

	Sleep(200)
	ConsoleWrite(@CRLF & "GET_CHARA_RECT")

	Local $TopLeft_X
	Local $BotRight_X
	Local $TopLeft_Y
	Local $BotRight_Y

	Local $time = TimerInit()


	$aCoord = PixelSearch($PLAYER_CLICK_POSITION[0], $PLAYER_CLICK_POSITION[1], $PLAYER_CLICK_POSITION[0] + 1, $PLAYER_CLICK_POSITION[1] + 350, 0x171717)
	While $aCoord == 0
		$aCoord = PixelSearch($PLAYER_CLICK_POSITION[0], $PLAYER_CLICK_POSITION[1], $PLAYER_CLICK_POSITION[0] + 1, $PLAYER_CLICK_POSITION[1] + 350, 0x171717)
		If $aCoord == 0 Then
			Return False
		EndIf
		Sleep(10)
	WEnd

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)


	Local $posY = $PLAYER_CLICK_POSITION[1] + 100
	Local $MessageFound = False
	$col1 = PixelSearch($PLAYER_CLICK_POSITION[0] + 7, $PLAYER_CLICK_POSITION[1] + 100, $PLAYER_CLICK_POSITION[0] + 7, $PLAYER_CLICK_POSITION[1] + 315, 0x000000)

;~ While $MessageFound == False And $posY < $PLAYER_CLICK_POSITION[1]+315
	While $posY < $PLAYER_CLICK_POSITION[1] + 315
		If $col1 <> 0 And _
				_PixelGetColor_GetPixel($vDC, $col1[0] + 1, $col1[1], $hDll) == "F7F7F7" And _
				_PixelGetColor_GetPixel($vDC, $col1[0] + 2, $col1[1], $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $col1[0] + 3, $col1[1], $hDll) == "F7F7F7" And _
				_PixelGetColor_GetPixel($vDC, $col1[0] + 4, $col1[1], $hDll) == "F7F7F7" And _
				_PixelGetColor_GetPixel($vDC, $col1[0] + 5, $col1[1], $hDll) == "F7F7F7" And _
				_PixelGetColor_GetPixel($vDC, $col1[0] + 6, $col1[1], $hDll) == "000000" Then
			$MessageFound = True
			ConsoleWrite(@CRLF & "Message String found: " & $col1[0] & "x" & $col1[1])
			ExitLoop
		EndIf

		ConsoleWrite(@CRLF & "Search <M> at y-position: " & $posY)
		If $col1 <> 0 And ($col1[1] > $PLAYER_CLICK_POSITION[1]) Then
			$posY = $col1[1] + 1
		Else
			$posY = $posY + 1
		EndIf

		$col1 = PixelSearch($PLAYER_CLICK_POSITION[0], $posY, $PLAYER_CLICK_POSITION[0] + 7, $PLAYER_CLICK_POSITION[1] + 315, 0x000000)

		If $posY >= $PLAYER_CLICK_POSITION[1] + 315 Then
			Return False
		EndIf

	WEnd

	ConsoleWrite(@CRLF & "Loop[1] #END")

	If $posY >= $PLAYER_CLICK_POSITION[1] + 315 Then
		Return False
	EndIf

	If $col1 == 0 Then
		Return False
	EndIf

	Local $color

	Local $width = 0
	While $color <> "121212"
		$color = _PixelGetColor_GetPixel($vDC, $PLAYER_CLICK_POSITION[0] + $width, $PLAYER_CLICK_POSITION[1] + 1, $hDll)
		$width = $width + 1
	WEnd


	Local $height = 0
	While $color <> "171717"
		$color = _PixelGetColor_GetPixel($vDC, $PLAYER_CLICK_POSITION[0] + 1, $PLAYER_CLICK_POSITION[1] + $height, $hDll)
		$height = $height + 1
	WEnd

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)


	ConsoleWrite(@CRLF & "RECTANGLE WIDTH & HEIGHT !!!! WIDTH: " & $width & "  HEIGHT: " & $height)
	;======= CHARACTER NAME RECTANGLE ===========

	$TopLeft_X = $col1[0] + 77
	$TopLeft_Y = $col1[1] - 4
	$BotRight_X = ($PLAYER_CLICK_POSITION[0] + $width) - 69
	$BotRight_Y = $TopLeft_Y + 9


	Local $tempRectangle[4] = [$TopLeft_X, $TopLeft_Y, $BotRight_X, $BotRight_Y]
	$PLAYER_NAME_RECTANGLE = $tempRectangle
	$PLAYER_NAME_WIDTH = $PLAYER_NAME_RECTANGLE[2] - $PLAYER_NAME_RECTANGLE[0]

	Local $diff = TimerDiff($time)
	ConsoleWrite(@CRLF & "======Func GetCharacterNameRectangle======")
	ConsoleWrite(@CRLF & "PLAYER_NAME_RECTANGLE = " & $PLAYER_NAME_RECTANGLE[0] & "x" & $PLAYER_NAME_RECTANGLE[1] & " => " & $PLAYER_NAME_RECTANGLE[2] & "x" & $PLAYER_NAME_RECTANGLE[3])
	ConsoleWrite(@CRLF & "Process Time: " & $diff)
	ConsoleWrite(@CRLF & "============================================")
	Return True

EndFunc   ;==>GetCharacterNameRectangle
Func GetCurrentUsername()
	Local $time = TimerInit()

	Local $result_capital
	Local $result_small

	Local $string_result
	Local $empty_string = 0

	$NAME_WIDTH = $PLAYER_NAME_RECTANGLE[2] - $PLAYER_NAME_RECTANGLE[0]

	$STRING_BLOCK_SCAN_X = $PLAYER_NAME_RECTANGLE[0]
	$STRING_BLOCK_SCAN_Y = $PLAYER_NAME_RECTANGLE[1]
	$CURRENT_CHARACTER_NAME = ""

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	While $STRING_BLOCK_SCAN_X < $PLAYER_NAME_RECTANGLE[2]
		If CheckCapitalLetters($STRING_BLOCK_SCAN_X, $STRING_BLOCK_SCAN_Y, $hDll, $vDC) == False Then
			If CheckSmallLetters($STRING_BLOCK_SCAN_X, $STRING_BLOCK_SCAN_Y, $hDll, $vDC) == False Then
				$empty_string = $empty_string + 1
				If $empty_string == 2 Then
					$CURRENT_CHARACTER_NAME = StringTrimRight($CURRENT_CHARACTER_NAME, 1)
					ExitLoop
				Else
					$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & " "
					$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 4
				EndIf
			Else
				$empty_string = 0
				ConsoleWrite(@CRLF & "$STRING_BLOCK_SCAN_X= " & $STRING_BLOCK_SCAN_X)
			EndIf
		Else
			$empty_string = 0
			ConsoleWrite(@CRLF & "$STRING_BLOCK_SCAN_X= " & $STRING_BLOCK_SCAN_X)
		EndIf
	WEnd

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	Local $diff = TimerDiff($time)
	ConsoleWrite(@CRLF & "================GetCurrentUsername================")
	ConsoleWrite(@CRLF & "CurrentCharacterName = " & $CURRENT_CHARACTER_NAME)
	ConsoleWrite(@CRLF & "CurrentCharacterLength = " & $NAME_WIDTH)
	ConsoleWrite(@CRLF & "Process Time: " & $diff)
	ConsoleWrite(@CRLF & "============================================")

	Return $CURRENT_CHARACTER_NAME
EndFunc   ;==>GetCurrentUsername
Func OpenChatChannel()
;~ 	Sleep(250)
	MouseClick($MOUSE_CLICK_LEFT, $PLAYER_NAME_RECTANGLE[0], $PLAYER_NAME_RECTANGLE[1] + 4, 1, 0)
EndFunc   ;==>OpenChatChannel


Func GetMoveObjectsWindowOkPosition()

	Local $tempX = ($BLACK_WINDOW_X + $BLACK_WINDOW_WIDTH) / 2
	Local $tempY = ($BLACK_WINDOW_Y + $BLACK_WINDOW_HEIGHT) / 2

	Local $tempPosition[2] = [$tempX + 30, $tempY + 45]

	$MOVE_OBJECT_OK_POSITION = $tempPosition

	ConsoleWrite(@CRLF & "======Func GetMoveObjectsWindowOkPosition======")
	ConsoleWrite(@CRLF & "MOVE_OBJECT_OK_POSITION = " & $MOVE_OBJECT_OK_POSITION[0] & "x" & $MOVE_OBJECT_OK_POSITION[1])
	ConsoleWrite(@CRLF & "============================================")
EndFunc   ;==>GetMoveObjectsWindowOkPosition

;========== GET RECTANGLES & POSITIONS FOR ACTION WINDOW ===============
Func GetTibiaActionWindowRectangle($log)

	If Not TibiaWindowIsFocused() Then
		ActivateTibiaWindow()
	EndIf

	GUICtrlSetData($status, "Init ActionWindow...")

	Local $time = TimerInit()

	$TIBIA_ACTION_WINDOW_RECTANGLE[3] = $CHAT_WINDOW_RECTANGLE[1] - 108

	Local $tempa = $CHAT_WINDOW_RECTANGLE[0] - 7
	Local $tempb = $CHAT_WINDOW_RECTANGLE[2] + 19

	Local $tempc = ($tempb - $tempa) / 2

	$TIBIA_ACTION_WINDOW_CENTRE_X = $tempa + $tempc



	$startScanX = $BLACK_WINDOW_X
	$startScanY = $BLACK_WINDOW_Y
	$endScanX = $TIBIA_ACTION_WINDOW_CENTRE_X
	$endScanY = $BLACK_WINDOW_Y + 40

	Local $LEFT_AW_unique_colors_detected = 0

	Local $aCoord = PixelSearch(0, 26, $endScanX, $endScanY, 0x181818) ;0x42415c unique blue hex color at mana bar
	If $aCoord <> 0 And _
			PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x181818) == 0 And _
			PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x171717) == 0 And _
			PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x161616) == 0 And _
			PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x151515) == 0 And _
			PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x171817) == 0 And _
			PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x141414) == 0 And _
			PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x1a1a1a) == 0 And _
			PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x151616) == 0 And _
			PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x131313) == 0 And _
			PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x1b1b1a) == 0 And _
			PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x1b1b1b) == 0 And _
			PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x191a19) == 0 And _
			PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x131413) == 0 And _
			PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x191919) == 0 Then
		$LEFT_AW_unique_colors_detected = 1
	Else
		Local $aCoord = PixelSearch(0, 26, $endScanX, $endScanY, 0x171717)
		If $aCoord <> 0 And _
				PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x181818) == 0 And _
				PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x171717) == 0 And _
				PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x161616) == 0 And _
				PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x151515) == 0 And _
				PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x171817) == 0 And _
				PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x141414) == 0 And _
				PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x1a1a1a) == 0 And _
				PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x151616) == 0 And _
				PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x131313) == 0 And _
				PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x1b1b1a) == 0 And _
				PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x1b1b1b) == 0 And _
				PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x191a19) == 0 And _
				PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x131413) == 0 And _
				PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x191919) == 0 Then
			$LEFT_AW_unique_colors_detected = 1
		Else
			Local $aCoord = PixelSearch(0, 26, $endScanX, $endScanY, 0x161616)
			If $aCoord <> 0 And _
					PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x181818) == 0 And _
					PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x171717) == 0 And _
					PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x161616) == 0 And _
					PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x151515) == 0 And _
					PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x171817) == 0 And _
					PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x141414) == 0 And _
					PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x1a1a1a) == 0 And _
					PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x151616) == 0 And _
					PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x131313) == 0 And _
					PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x1b1b1a) == 0 And _
					PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x1b1b1b) == 0 And _
					PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x191a19) == 0 And _
					PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x131413) == 0 And _
					PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x191919) == 0 Then
				$LEFT_AW_unique_colors_detected = 1
			Else
				Local $aCoord = PixelSearch(0, 26, $endScanX, $endScanY, 0x151515)
				If $aCoord <> 0 And _
						PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x181818) == 0 And _
						PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x171717) == 0 And _
						PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x161616) == 0 And _
						PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x151515) == 0 And _
						PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x171817) == 0 And _
						PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x141414) == 0 And _
						PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x1a1a1a) == 0 And _
						PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x151616) == 0 And _
						PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x131313) == 0 And _
						PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x1b1b1a) == 0 And _
						PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x1b1b1b) == 0 And _
						PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x191a19) == 0 And _
						PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x131413) == 0 And _
						PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x191919) == 0 Then
					$LEFT_AW_unique_colors_detected = 1
				Else
					Local $aCoord = PixelSearch(0, 26, $endScanX, $endScanY, 0x141414)
					If $aCoord <> 0 And _
							PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x181818) == 0 And _
							PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x171717) == 0 And _
							PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x161616) == 0 And _
							PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x151515) == 0 And _
							PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x171817) == 0 And _
							PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x141414) == 0 And _
							PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x1a1a1a) == 0 And _
							PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x151616) == 0 And _
							PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x131313) == 0 And _
							PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x1b1b1a) == 0 And _
							PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x1b1b1b) == 0 And _
							PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x191a19) == 0 And _
							PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x131413) == 0 And _
							PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x191919) == 0 Then
						$LEFT_AW_unique_colors_detected = 1
					Else
						Local $aCoord = PixelSearch(0, 26, $endScanX, $endScanY, 0x131313)
						If $aCoord <> 0 And _
								PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x181818) == 0 And _
								PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x171717) == 0 And _
								PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x161616) == 0 And _
								PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x151515) == 0 And _
								PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x171817) == 0 And _
								PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x141414) == 0 And _
								PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x1a1a1a) == 0 And _
								PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x1b1b1b) == 0 And _
								PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x151616) == 0 And _
								PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x131313) == 0 And _
								PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x1b1b1a) == 0 And _
								PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x191a19) == 0 And _
								PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x131413) == 0 And _
								PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x191919) == 0 Then
							$LEFT_AW_unique_colors_detected = 1
						Else
							Local $aCoord = PixelSearch(0, 26, $endScanX, $endScanY, 0x151616)
							If $aCoord <> 0 And _
									PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x181818) == 0 And _
									PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x171717) == 0 And _
									PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x161616) == 0 And _
									PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x151515) == 0 And _
									PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x171817) == 0 And _
									PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x141414) == 0 And _
									PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x1a1a1a) == 0 And _
									PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x1b1b1b) == 0 And _
									PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x151616) == 0 And _
									PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x131313) == 0 And _
									PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x1b1b1a) == 0 And _
									PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x191a19) == 0 And _
									PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x131413) == 0 And _
									PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x191919) == 0 Then
								$LEFT_AW_unique_colors_detected = 1
							Else
								Local $aCoord = PixelSearch(0, 26, $endScanX, $endScanY, 0x1a1a1a)
								If $aCoord <> 0 And _
										PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x181818) == 0 And _
										PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x171717) == 0 And _
										PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x161616) == 0 And _
										PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x151515) == 0 And _
										PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x171817) == 0 And _
										PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x141414) == 0 And _
										PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x1a1a1a) == 0 And _
										PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x151616) == 0 And _
										PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x131313) == 0 And _
										PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x1b1b1a) == 0 And _
										PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x1b1b1b) == 0 And _
										PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x191a19) == 0 And _
										PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x131413) == 0 And _
										PixelSearch($aCoord[0] - 1, $aCoord[1], $aCoord[0] - 1, $aCoord[1], 0x191919) == 0 Then
									$LEFT_AW_unique_colors_detected = 1
								EndIf
							EndIf
						EndIf
					EndIf
				EndIf
			EndIf
		EndIf
	EndIf

	Local $diff = TimerDiff($time)

	If $LEFT_AW_unique_colors_detected == 1 Then

		ConsoleWrite(@CRLF & $aCoord[0] & "x" & $aCoord[1] & ": IT'S A MATCH !!! ")

		$TIBIA_ACTION_WINDOW_TOPLEFT = $aCoord[0]
		$TIBIA_ACTION_WINDOW_RECTANGLE[0] = $aCoord[0] + 1
		$TIBIA_ACTION_WINDOW_RECTANGLE[1] = $aCoord[1] + 1

		$TIBIA_ACTION_WINDOW_RECTANGLE[2] = $TIBIA_ACTION_WINDOW_CENTRE_X + ($TIBIA_ACTION_WINDOW_CENTRE_X - $TIBIA_ACTION_WINDOW_RECTANGLE[0]) - 1

		$TILE_WIDTH = ($TIBIA_ACTION_WINDOW_RECTANGLE[2] - $TIBIA_ACTION_WINDOW_RECTANGLE[0]) / 15
		$TILE_HEIGHT = ($TIBIA_ACTION_WINDOW_RECTANGLE[3] - $TIBIA_ACTION_WINDOW_RECTANGLE[1]) / 11

		$TIBIA_ACTION_WINDOW_CENTRE_Y = $TIBIA_ACTION_WINDOW_RECTANGLE[1] + (($TIBIA_ACTION_WINDOW_RECTANGLE[3] - $TIBIA_ACTION_WINDOW_RECTANGLE[1]) / 2)

		GUICtrlSetData($status, "Successful")

		If $log == 1 Then
			Local $hFileOpen = FileOpen($sFilePath, 1)
			FileWrite($hFileOpen, @CRLF & "====== Initialize GetTibiaActionWindowRectangle ======")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |TibiaActionWindow.found = True")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |ActionWindow")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |CENTRE: " & $TIBIA_ACTION_WINDOW_CENTRE_X & " x " & $TIBIA_ACTION_WINDOW_CENTRE_Y)
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |X: " & $TIBIA_ACTION_WINDOW_RECTANGLE[0] & " - " & $TIBIA_ACTION_WINDOW_RECTANGLE[2])
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Y: " & $TIBIA_ACTION_WINDOW_RECTANGLE[1] & " - " & $TIBIA_ACTION_WINDOW_RECTANGLE[3])
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |TILE_WIDTH: " & $TILE_WIDTH)
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |TILE_HEIGHT: " & $TILE_HEIGHT)
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
			FileWrite($hFileOpen, @CRLF)
			FileClose($hFileOpen)
		EndIf
	Else

		GUICtrlSetData($status, "Failed")

		If $log == 1 Then
			Local $hFileOpen = FileOpen($sFilePath, 1)
			FileWrite($hFileOpen, @CRLF & "====== Initialize GetTibiaActionWindowRectangle ======")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |TibiaActionWindow.found = False")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |ActionWindow")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |CENTRE: " & $TIBIA_ACTION_WINDOW_CENTRE_X & " x " & $TIBIA_ACTION_WINDOW_CENTRE_Y)
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |X: " & $TIBIA_ACTION_WINDOW_RECTANGLE[0] & " - " & $TIBIA_ACTION_WINDOW_RECTANGLE[2])
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Y: " & $TIBIA_ACTION_WINDOW_RECTANGLE[1] & " - " & $TIBIA_ACTION_WINDOW_RECTANGLE[3])
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |TILE_WIDTH: " & $TILE_WIDTH)
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |TILE_HEIGHT: " & $TILE_HEIGHT)
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
			FileWrite($hFileOpen, @CRLF)
			FileClose($hFileOpen)
		EndIf
	EndIf

	ConsoleWrite(@CRLF & "======Func GetTibiaActionWindowRectangle======")
	ConsoleWrite(@CRLF & "TIBIA_ACTION_WINDOW_RECTANGLE = " & $TIBIA_ACTION_WINDOW_RECTANGLE[0] & "x" & $TIBIA_ACTION_WINDOW_RECTANGLE[1] & " => " & $TIBIA_ACTION_WINDOW_RECTANGLE[2] & "x" & $TIBIA_ACTION_WINDOW_RECTANGLE[3])
	ConsoleWrite(@CRLF & "TIBIA_ACTION_WINDOW_CENTRE = " & $TIBIA_ACTION_WINDOW_CENTRE_X & "x" & $TIBIA_ACTION_WINDOW_CENTRE_Y)
	ConsoleWrite(@CRLF & "TILE_WIDTH = " & $TILE_WIDTH)
	ConsoleWrite(@CRLF & "TILE_HEIGHT = " & $TILE_HEIGHT)
	ConsoleWrite(@CRLF & "Process Time: " & $diff)
	ConsoleWrite(@CRLF & "============================================" & @CRLF)
EndFunc   ;==>GetTibiaActionWindowRectangle
Func GetCharacterTileRectangle($log)

	GUICtrlSetData($status, "Init CharacterTile..")

	Local $time = TimerInit()

	Local $top_left_x = StringFormat("%.0f", $TIBIA_ACTION_WINDOW_RECTANGLE[0] + ($TILE_WIDTH * 7))
	Local $top_left_y = StringFormat("%.0f", $TIBIA_ACTION_WINDOW_RECTANGLE[1] + ($TILE_HEIGHT * 5))
	Local $bot_right_x = StringFormat("%.0f", $top_left_x + $TILE_WIDTH)
	Local $bot_right_y = StringFormat("%.0f", $top_left_y + $TILE_HEIGHT)


	Local $tempArray[4] = [$top_left_x, $top_left_y, $bot_right_x, $bot_right_y]
	$CHARACTER_TILE_RECTANGLE = $tempArray

	Local $diff = TimerDiff($time)

	If $CHARACTER_TILE_RECTANGLE[0] <> 0 And $CHARACTER_TILE_RECTANGLE[1] <> 0 Then
		GUICtrlSetData($status, "Successful")
		If $log == 1 Then
			Local $hFileOpen = FileOpen($sFilePath, 1)
			FileWrite($hFileOpen, @CRLF & "====== Initialize GetCharacterTileRectangle ======")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |CharacterRectangle.found = True")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |CharacterRectangle")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |X: " & $CHARACTER_TILE_RECTANGLE[0] & " - " & $CHARACTER_TILE_RECTANGLE[2])
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Y: " & $CHARACTER_TILE_RECTANGLE[1] & " - " & $CHARACTER_TILE_RECTANGLE[3])
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
			FileWrite($hFileOpen, @CRLF)
			FileClose($hFileOpen)
		EndIf
	Else
		GUICtrlSetData($status, "Failed")
		If $log == 1 Then
			Local $hFileOpen = FileOpen($sFilePath, 1)
			FileWrite($hFileOpen, @CRLF & "====== Initialize GetCharacterTileRectangle ======")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |CharacterRectangle.found = False")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |CharacterRectangle")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |X: " & $CHARACTER_TILE_RECTANGLE[0] & " - " & $CHARACTER_TILE_RECTANGLE[2])
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Y: " & $CHARACTER_TILE_RECTANGLE[1] & " - " & $CHARACTER_TILE_RECTANGLE[3])
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
			FileWrite($hFileOpen, @CRLF)
			FileClose($hFileOpen)
		EndIf
	EndIf

	ConsoleWrite(@CRLF & "===== Character Tile Rectangle =====")
	ConsoleWrite(@CRLF & $CHARACTER_TILE_RECTANGLE[0] & " => " & $CHARACTER_TILE_RECTANGLE[2])
	ConsoleWrite(@CRLF & $CHARACTER_TILE_RECTANGLE[1] & " => " & $CHARACTER_TILE_RECTANGLE[3])
	ConsoleWrite(@CRLF & "Process Time: " & $diff)
	ConsoleWrite(@CRLF & "============================================" & @CRLF)
EndFunc   ;==>GetCharacterTileRectangle
Func GetPlayerTileRectangle($log)

	GUICtrlSetData($status, "Init PlayerTile..")

	Local $time = TimerInit()

	If $BLACK_WEST Then
		Local $top_left_x = StringFormat("%.0f", $TIBIA_ACTION_WINDOW_RECTANGLE[0] + ($TILE_WIDTH * 9))
		Local $top_left_y = StringFormat("%.0f", $TIBIA_ACTION_WINDOW_RECTANGLE[1] + ($TILE_HEIGHT * 5))
		Local $bot_right_x = StringFormat("%.0f", $top_left_x + $TILE_WIDTH)
		Local $bot_right_y = StringFormat("%.0f", $top_left_y + $TILE_HEIGHT)
	ElseIf $BLACK_EAST Then
		Local $top_left_x = StringFormat("%.0f", $TIBIA_ACTION_WINDOW_RECTANGLE[0] + ($TILE_WIDTH * 5))
		Local $top_left_y = StringFormat("%.0f", $TIBIA_ACTION_WINDOW_RECTANGLE[1] + ($TILE_HEIGHT * 5))
		Local $bot_right_x = StringFormat("%.0f", $top_left_x + $TILE_WIDTH)
		Local $bot_right_y = StringFormat("%.0f", $top_left_y + $TILE_HEIGHT)
	EndIf

	Local $tempArray[4] = [$top_left_x, $top_left_y, $bot_right_x, $bot_right_y]
	$PLAYER_TILE_RECTANGLE = $tempArray

	Local $diff = TimerDiff($time)

	If $PLAYER_TILE_RECTANGLE[0] <> 0 And $PLAYER_TILE_RECTANGLE[1] <> 0 Then
		GUICtrlSetData($status, "Successful")
		If $log == 1 Then
			Local $hFileOpen = FileOpen($sFilePath, 1)
			FileWrite($hFileOpen, @CRLF & "====== Initialize PlayerTileRectangle ======")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |PlayerRectangle.found = True")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |PlayerRectangle")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |X: " & $PLAYER_TILE_RECTANGLE[0] & " - " & $PLAYER_TILE_RECTANGLE[2])
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Y: " & $PLAYER_TILE_RECTANGLE[1] & " - " & $PLAYER_TILE_RECTANGLE[3])
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
			FileWrite($hFileOpen, @CRLF)
			FileClose($hFileOpen)
		EndIf
	Else
		GUICtrlSetData($status, "Failed")
		If $log == 1 Then
			Local $hFileOpen = FileOpen($sFilePath, 1)
			FileWrite($hFileOpen, @CRLF & "====== Initialize PlayerTileRectangle ======")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |PlayerRectangle.found = False")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |PlayerRectangle")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |X: " & $PLAYER_TILE_RECTANGLE[0] & " - " & $PLAYER_TILE_RECTANGLE[2])
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Y: " & $PLAYER_TILE_RECTANGLE[1] & " - " & $PLAYER_TILE_RECTANGLE[3])
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
			FileWrite($hFileOpen, @CRLF)
			FileClose($hFileOpen)
		EndIf
	EndIf

	ConsoleWrite(@CRLF & "===== PlayerTileRectangle =====")
	ConsoleWrite(@CRLF & $PLAYER_TILE_RECTANGLE[0] & " => " & $PLAYER_TILE_RECTANGLE[2])
	ConsoleWrite(@CRLF & $PLAYER_TILE_RECTANGLE[1] & " => " & $PLAYER_TILE_RECTANGLE[3])
	ConsoleWrite(@CRLF & "Process Time: " & $diff)
	ConsoleWrite(@CRLF & "============================================" & @CRLF)
EndFunc   ;==>GetPlayerTileRectangle
Func GetPlayerSwitchCheckSum()
	$tempCheckSum = PixelChecksum($PLAYER_TILE_RECTANGLE[0], $PLAYER_TILE_RECTANGLE[1], $PLAYER_TILE_RECTANGLE[2], $PLAYER_TILE_RECTANGLE[3])
	Return $tempCheckSum
EndFunc   ;==>GetPlayerSwitchCheckSum
Func SaveCurrentPlayerCheckSum($log)

	GUICtrlSetData($status, "Init PlayerSwitchCheckSum..")

	Local $time = TimerInit()

	Local $tempCheckSum

	ConsoleWrite(@CRLF & "===== CHECKSUM COLLECTION =====")
	Local $time = TimerInit()



	While TimerDiff($time) <= 1500
		$tempCheckSum = GetPlayerSwitchCheckSum()
		ConsoleWrite(@CRLF & "CHECKSUM: " & $tempCheckSum)

		$array_add = 1
		For $y = 0 To (UBound($CHECKSUMS) - 1)
			If $CHECKSUMS[$y] == $tempCheckSum Then
				$array_add = 0
			EndIf
		Next

		If $array_add == 1 And Not CheckIfPlayer() Then
			_ArrayAdd($CHECKSUMS, $tempCheckSum)
			$PLAYER_SWITCH_CHECKSUM = $tempCheckSum
		EndIf
	WEnd

	Local $diff = TimerDiff($time)

	If $CHECKSUMS[0] <> 0 Then
		GUICtrlSetData($status, "Successful")
		If $log == 1 Then
			Local $hFileOpen = FileOpen($sFilePath, 1)
			FileWrite($hFileOpen, @CRLF & "====== Initialize PlayerSwitchCheckSum ======")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |PlayerSwitchCheckSum.found = True")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |PlayerSwitchCheckSum")

			For $a = 0 To (UBound($CHECKSUMS) - 1)
				FileWrite($hFileOpen, @CRLF & _NowTime() & " |CheckSum[" & $a & "]: " & $CHECKSUMS[$a])
			Next

			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
			FileWrite($hFileOpen, @CRLF)
			FileClose($hFileOpen)
		EndIf
	Else
		GUICtrlSetData($status, "Failed")
		If $log == 1 Then
			Local $hFileOpen = FileOpen($sFilePath, 1)
			FileWrite($hFileOpen, @CRLF & "====== Initialize PlayerSwitchCheckSum ======")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |PlayerSwitchCheckSum.found = False")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |PlayerSwitchCheckSum")

			For $a = 0 To (UBound($CHECKSUMS) - 1)
				FileWrite($hFileOpen, @CRLF & _NowTime() & " |CheckSum[" & $a & "]: " & $CHECKSUMS[$a])
			Next

			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
			FileWrite($hFileOpen, @CRLF)
			FileClose($hFileOpen)
		EndIf
	EndIf
;~ 	_ArrayDisplay($CHECKSUMS)

	ConsoleWrite(@CRLF & "===== SaveCurrentPlayerCheckSum =====")
	For $a = 0 To (UBound($CHECKSUMS) - 1)
		ConsoleWrite(@CRLF & $CHECKSUMS[$a])
	Next
	ConsoleWrite(@CRLF & "Process Time: " & $diff)
	ConsoleWrite(@CRLF & "============================================" & @CRLF)
EndFunc   ;==>SaveCurrentPlayerCheckSum
Func GetPlayerClickPosition($log)

	GUICtrlSetData($status, "Init PlayerClickPosition...")

	Local $time = TimerInit()

	If $BLACK_WEST Then
		Local $tempX = $TIBIA_ACTION_WINDOW_CENTRE_X + ($TILE_WIDTH * 2)

		$PLAYER_CLICK_POSITION[0] = StringFormat("%.0f", $tempX)
		$PLAYER_CLICK_POSITION[1] = $TIBIA_ACTION_WINDOW_CENTRE_Y
	ElseIf $BLACK_EAST Then
		Local $tempX = $TIBIA_ACTION_WINDOW_CENTRE_X - ($TILE_WIDTH * 2)

		$PLAYER_CLICK_POSITION[0] = StringFormat("%.0f", $tempX)
		$PLAYER_CLICK_POSITION[1] = $TIBIA_ACTION_WINDOW_CENTRE_Y
	EndIf

	Local $diff = TimerDiff($time)

	If $PLAYER_CLICK_POSITION[0] <> 0 And $PLAYER_CLICK_POSITION[1] <> 0 Then
		If $log == 1 Then
			Local $hFileOpen = FileOpen($sFilePath, 1)
			FileWrite($hFileOpen, @CRLF & "====== Initialize PlayerClickPosition... ======")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |PlayerClickPosition.found = True")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |PlayerClickPosition")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Posittion: " & $PLAYER_CLICK_POSITION[0] & " x " & $PLAYER_CLICK_POSITION[1])
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
			FileWrite($hFileOpen, @CRLF)
			FileClose($hFileOpen)
		EndIf
	Else
		If $log == 1 Then
			Local $hFileOpen = FileOpen($sFilePath, 1)
			FileWrite($hFileOpen, @CRLF & "====== Initialize PlayerClickPosition... ======")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |PlayerClickPosition.found = False")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |PlayerClickPosition")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Posittion: " & $PLAYER_CLICK_POSITION[0] & " x " & $PLAYER_CLICK_POSITION[1])
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
			FileWrite($hFileOpen, @CRLF)
			FileClose($hFileOpen)
		EndIf
	EndIf

	ConsoleWrite(@CRLF & "===== GetPlayerClickPosition =====")
	ConsoleWrite(@CRLF & $PLAYER_CLICK_POSITION[0] & "x" & $PLAYER_CLICK_POSITION[1])
	ConsoleWrite(@CRLF & "Process Time: " & $diff)
	ConsoleWrite(@CRLF & "============================================" & @CRLF)
EndFunc   ;==>GetPlayerClickPosition
Func GetPlayerLockerDragPosition($log)

	GUICtrlSetData($status, "Init PlayerLockerDragPosition")

	Local $time = TimerInit()

	If $BLACK_NORTH Then
		If $BLACK_WEST Then
			Local $tempX = $TIBIA_ACTION_WINDOW_CENTRE_X + ($TILE_WIDTH * 2)
			Local $tempY = $TIBIA_ACTION_WINDOW_CENTRE_Y - $TILE_HEIGHT
		ElseIf $BLACK_EAST Then
			Local $tempX = $TIBIA_ACTION_WINDOW_CENTRE_X - ($TILE_WIDTH * 2)
			Local $tempY = $TIBIA_ACTION_WINDOW_CENTRE_Y - $TILE_HEIGHT
		EndIf
	ElseIf $BLACK_SOUTH Then
		If $BLACK_WEST Then
			Local $tempX = $TIBIA_ACTION_WINDOW_CENTRE_X + ($TILE_WIDTH * 2)
			Local $tempY = $TIBIA_ACTION_WINDOW_CENTRE_Y + $TILE_HEIGHT
		ElseIf $BLACK_EAST Then
			Local $tempX = $TIBIA_ACTION_WINDOW_CENTRE_X - ($TILE_WIDTH * 2)
			Local $tempY = $TIBIA_ACTION_WINDOW_CENTRE_Y + $TILE_HEIGHT
		EndIf
	EndIf

	$PLAYER_LOCKER_POSITION[0] = StringFormat("%.0f", $tempX)
	$PLAYER_LOCKER_POSITION[1] = StringFormat("%.0f", $tempY)

	Local $diff = TimerDiff($time)

	If $PLAYER_LOCKER_POSITION[0] <> 0 And $PLAYER_LOCKER_POSITION[1] <> 0 Then
		If $log == 1 Then
			Local $hFileOpen = FileOpen($sFilePath, 1)
			FileWrite($hFileOpen, @CRLF & "====== Initialize PlayerLockerDragPosition ======")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |PlayerLockerDragPosition.found = True")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |PlayerLockerDragPosition")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Posittion: " & $PLAYER_LOCKER_POSITION[0] & " x " & $PLAYER_LOCKER_POSITION[1])
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
			FileWrite($hFileOpen, @CRLF)
			FileClose($hFileOpen)
		EndIf
	Else
		If $log == 1 Then
			Local $hFileOpen = FileOpen($sFilePath, 1)
			FileWrite($hFileOpen, @CRLF & "====== Initialize PlayerLockerDragPosition ======")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |PlayerLockerDragPosition.found = False")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |PlayerLockerDragPosition")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Posittion: " & $PLAYER_LOCKER_POSITION[0] & " x " & $PLAYER_LOCKER_POSITION[1])
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
			FileWrite($hFileOpen, @CRLF)
			FileClose($hFileOpen)
		EndIf
	EndIf

	ConsoleWrite(@CRLF & "===== GetPlayerLockerDragPosition =====")
	ConsoleWrite(@CRLF & $PLAYER_LOCKER_POSITION[0] & "x" & $PLAYER_LOCKER_POSITION[1])
	ConsoleWrite(@CRLF & "Process Time: " & $diff)
	ConsoleWrite(@CRLF & "============================================" & @CRLF)
EndFunc   ;==>GetPlayerLockerDragPosition

;========== GET RECTANGLES & POSITIONS FOR CHAT WINDOW ===============
Func GetChatWindowRectangle($log)

	If Not TibiaWindowIsFocused() Then
		ActivateTibiaWindow()
	EndIf

	Local $time = TimerInit()

	GUICtrlSetData($status, "Init ChatWindowRectangle...")

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $startScanX = $BLACK_WINDOW_X
	Local $startScanY = $BLACK_WINDOW_Y
	Local $endScanX = $BLACK_WINDOW_X + 30
	Local $endScanY = $BLACK_WINDOW_Y + $BLACK_WINDOW_HEIGHT

	$unique_colors_detected = 0

	While $unique_colors_detected == 0
		Local $aCoord = PixelSearch($startScanX, $startScanY, $endScanX, $endScanY, 0xa1b1b1c)
		If $aCoord <> 0 And _
				_PixelGetColor_GetPixel($vDC, $aCoord[0] - 1, $aCoord[1], $hDll) == "3F3F3F" And _
				_PixelGetColor_GetPixel($vDC, $aCoord[0] - 1, $aCoord[1] - 1, $hDll) == "3F3F3F" Then
			$unique_colors_detected = 1
			ConsoleWrite(@CRLF & $aCoord[0] & "x" & $aCoord[1] & ": IT'S A MATCH !!! ")
			ExitLoop
		Else
			If $aCoord <> 0 Then
				Local $temp[2] = [$aCoord[0], $aCoord[1]]
				ConsoleWrite(@CRLF & $aCoord[0] & "x" & $aCoord[1] & ": NO MATCH")
				$startScanX = $aCoord[0] + 1
			Else
				$startScanX = $BLACK_WINDOW_X
				$startScanY = $endScanY
				$endScanY = $endScanY + 1
			EndIf

			$unique_colors_detected = 0
		EndIf
	WEnd



	If $unique_colors_detected == 1 Then
		Local $PosX = $aCoord[0]
		Local $posY = $aCoord[1]

		Local $TopLeft_X = $PosX + 1
		Local $TopLeft_Y = $posY + 1
	EndIf

	$startScanX = $PosX
	$startScanY = $posY
	$endScanX = $BLACK_WINDOW_X + $BLACK_WINDOW_WIDTH - 180
	$endScanY = $BLACK_WINDOW_Y + $BLACK_WINDOW_HEIGHT

	$unique_colors_detected = 0

	While $unique_colors_detected == 0
		Local $aCoord = PixelSearch($startScanX, $startScanY, $endScanX, $endScanY, 0x171717)
		If $aCoord <> 0 And _
				_PixelGetColor_GetPixel($vDC, $aCoord[0] + 1, $aCoord[1], $hDll) == "171717" And _
				_PixelGetColor_GetPixel($vDC, $aCoord[0] + 2, $aCoord[1], $hDll) == "171717" And _
				_PixelGetColor_GetPixel($vDC, $aCoord[0] + 2, $aCoord[1] - 1, $hDll) == "151515" And _
				_PixelGetColor_GetPixel($vDC, $aCoord[0] + 2, $aCoord[1] - 2, $hDll) == "161616" Then
			$unique_colors_detected = 1
			ConsoleWrite(@CRLF & $aCoord[0] & "x" & $aCoord[1] & ": IT'S A MATCH !!! ")
			ExitLoop
		Else
			If $aCoord <> 0 Then
				ConsoleWrite(@CRLF & $aCoord[0] & "x" & $aCoord[1] & ": NO MATCH")
				$startScanX = $aCoord[0] + 1
			Else
				$startScanX = $BLACK_WINDOW_X
				$startScanY = $endScanY
				$endScanY = $endScanY + 1
			EndIf
			$unique_colors_detected = 0
		EndIf
	WEnd

	Local $diff = TimerDiff($time)

	If $unique_colors_detected == 1 Then
		Local $PosX = $aCoord[0] - 13
		Local $posY = $aCoord[1] - 3

		;************** Rectangle ******************
		Local $BotRight_X = $PosX
		Local $BotRight_Y = $posY

		Local $Rectangle[4] = [$TopLeft_X, $TopLeft_Y, $BotRight_X, $BotRight_Y]

		$CHAT_WINDOW_RECTANGLE = $Rectangle

		GUICtrlSetData($status, "Successful")

		If $log == 1 Then
			Local $hFileOpen = FileOpen($sFilePath, 1)
			FileWrite($hFileOpen, @CRLF & "====== Initialize ChatRectangle ======")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |ChatWindow.found = True")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |ChatWindowRectangle")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |X: " & $CHAT_WINDOW_RECTANGLE[0] & " - " & $CHAT_WINDOW_RECTANGLE[2])
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Y: " & $CHAT_WINDOW_RECTANGLE[1] & " - " & $CHAT_WINDOW_RECTANGLE[3])
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
			FileWrite($hFileOpen, @CRLF)
			FileClose($hFileOpen)
		EndIf
	Else
		GUICtrlSetData($status, "Failed")

		If $log == 1 Then
			Local $hFileOpen = FileOpen($sFilePath, 1)
			FileWrite($hFileOpen, @CRLF & "====== Initialize ChatRectangle ======")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |ChatWindow.found = False")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |ChatWindowRectangle")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |X: " & $CHAT_WINDOW_RECTANGLE[0] & " - " & $CHAT_WINDOW_RECTANGLE[2])
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Y: " & $CHAT_WINDOW_RECTANGLE[1] & " - " & $CHAT_WINDOW_RECTANGLE[3])
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
			FileWrite($hFileOpen, @CRLF)
			FileClose($hFileOpen)
		EndIf
	EndIf



	ConsoleWrite(@CRLF & "======Func GetChatWindowRectangle======")
	ConsoleWrite(@CRLF & "CHAT_WINDOW_RECTANGLE = " & $CHAT_WINDOW_RECTANGLE[0] & "x" & $CHAT_WINDOW_RECTANGLE[1] & " => " & $CHAT_WINDOW_RECTANGLE[2] & "x" & $CHAT_WINDOW_RECTANGLE[3])
	ConsoleWrite(@CRLF & "Process Time: " & $diff)
	ConsoleWrite(@CRLF & "============================================")

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)
EndFunc   ;==>GetChatWindowRectangle
Func GetChatWindowCheckSum()
	Local $tempCheckSum = PixelChecksum($CHAT_WINDOW_RECTANGLE[0], $CHAT_WINDOW_RECTANGLE[1], $CHAT_WINDOW_RECTANGLE[2], $CHAT_WINDOW_RECTANGLE[3])
	Return $tempCheckSum
EndFunc   ;==>GetChatWindowCheckSum
Func SaveCurrentChatWindowCheckSum($log)

	Local $time = TimerInit()

	$CHAT_WINDOW_CHECKSUM = GetChatWindowCheckSum()

	Local $diff = TimerDiff($time)

	If $log == 1 Then
		If $CHAT_WINDOW_CHECKSUM <> 0 Then
			Local $hFileOpen = FileOpen($sFilePath, 1)
			FileWrite($hFileOpen, @CRLF & "====== Initialize ChatWindowCheckSum ======")
			FileWrite($hFileOpen, @CRLF & _NowTime() & "ChatWindowCheckSum.found = True")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |ChatWindowCheckSum")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |CheckSum: " & $CHAT_WINDOW_CHECKSUM)
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
			FileWrite($hFileOpen, @CRLF)
			FileClose($hFileOpen)
		Else
			Local $hFileOpen = FileOpen($sFilePath, 1)
			FileWrite($hFileOpen, @CRLF & "====== Initialize ChatWindowCheckSum ======")
			FileWrite($hFileOpen, @CRLF & _NowTime() & "ChatWindowCheckSum.found = False")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |ChatWindowCheckSum")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |CheckSum: " & $CHAT_WINDOW_CHECKSUM)
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
			FileWrite($hFileOpen, @CRLF)
			FileClose($hFileOpen)
		EndIf
	EndIf

EndFunc   ;==>SaveCurrentChatWindowCheckSum
Func GetCurrentMessagePosition($log)
	ConsoleWrite(@CRLF & "Start GetCurrentMessagePosition()")

	Local $time = TimerInit()

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $startScanX = $CHAT_WINDOW_RECTANGLE[0] + $PLAYER_NAME_WIDTH
	Local $startScanY = $CHAT_WINDOW_RECTANGLE[3] - 10
	Local $endScanX = $CHAT_WINDOW_RECTANGLE[2]
	Local $endScanY = $CHAT_WINDOW_RECTANGLE[3]

	$unique_colors_detected = 0

	While $unique_colors_detected == 0
		Local $aCoord = PixelSearch($startScanX, $startScanY, $endScanX, $endScanY, 0x60f8f8)
		If $aCoord <> 0 And _
				_PixelGetColor_GetPixel($vDC, $aCoord[0] + 1, $aCoord[1], $hDll) == "60F8F8" And _
				_PixelGetColor_GetPixel($vDC, $aCoord[0] + 2, $aCoord[1], $hDll) == "60F8F8" And _
				_PixelGetColor_GetPixel($vDC, $aCoord[0] + 3, $aCoord[1], $hDll) == "60F8F8" And _
				_PixelGetColor_GetPixel($vDC, $aCoord[0] + 2, $aCoord[1] + 1, $hDll) == "60F8F8" And _
				_PixelGetColor_GetPixel($vDC, $aCoord[0] + 2, $aCoord[1] + 2, $hDll) == "60F8F8" Then
			$unique_colors_detected = 1
			ConsoleWrite(@CRLF & $aCoord[0] & "x" & $aCoord[1] & ": IT'S A MATCH !!! ")
			ExitLoop
		Else
			If $aCoord <> 0 Then
				ConsoleWrite(@CRLF & $aCoord[0] & "x" & $aCoord[1] & ": NO MATCH")
				$startScanX = $aCoord[0] + 1
			Else
				$startScanX = $BLACK_WINDOW_X
				$startScanY = $endScanY
				$endScanY = $endScanY + 1
			EndIf
			$unique_colors_detected = 0
		EndIf
	WEnd

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	Local $diff = TimerDiff($time)

	If $unique_colors_detected == 1 Then
		Local $PosX = $aCoord[0] + 14
		Local $posY = $aCoord[1]

		;************** Rectangle ******************
		Local $TopLeft_X = $PosX
		Local $TopLeft_Y = $posY

		Local $BotRight_X = $CHAT_WINDOW_RECTANGLE[2]
		Local $BotRight_Y = $CHAT_WINDOW_RECTANGLE[3]


		Local $Rectangle[4] = [$TopLeft_X, $TopLeft_Y, $BotRight_X, $BotRight_Y]

		$MSG_RECTANGLE = $Rectangle


		If $log == 1 Then
			Local $hFileOpen = FileOpen($sFilePath, 1)
			FileWrite($hFileOpen, @CRLF & "====== Initialize MessageRectangle ======")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |MessageRectangle.found = True")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |MessageRectangle")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |X: " & $MSG_RECTANGLE[0] & " - " & $MSG_RECTANGLE[2])
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Y: " & $MSG_RECTANGLE[1] & " - " & $MSG_RECTANGLE[3])
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
			FileWrite($hFileOpen, @CRLF)
			FileClose($hFileOpen)
		EndIf

	Else

		If $log == 1 Then
			Local $hFileOpen = FileOpen($sFilePath, 1)
			FileWrite($hFileOpen, @CRLF & "====== Initialize MessageRectangle ======")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |MessageRectangle.found = False")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |MessageRectangle")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |X: " & $MSG_RECTANGLE[0] & " - " & $MSG_RECTANGLE[2])
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Y: " & $MSG_RECTANGLE[1] & " - " & $MSG_RECTANGLE[3])
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
			FileWrite($hFileOpen, @CRLF)
			FileClose($hFileOpen)
		EndIf
	EndIf


	ConsoleWrite(@CRLF & "======Func GetCurrentMessagePosition======")
	ConsoleWrite(@CRLF & "MSG_RECTANGLE = " & $MSG_RECTANGLE[0] & "x" & $MSG_RECTANGLE[1] & " => " & $MSG_RECTANGLE[2] & "x" & $MSG_RECTANGLE[3])
	ConsoleWrite(@CRLF & "Process Time: " & $diff)
	ConsoleWrite(@CRLF & "============================================")

EndFunc   ;==>GetCurrentMessagePosition

;==== Counter ====
;==== Counter ====
Func GetBrowseFieldCounterPosition($log)

	If Not TibiaWindowIsFocused() Then
		ActivateTibiaWindow()
	EndIf

	GUICtrlSetData($status, "Init Counter...")

	Local $time = TimerInit()

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $startScanX = $CHAT_WINDOW_RECTANGLE[2]
	Local $startScanY = $BLACK_WINDOW_Y
	Local $endScanX = $BLACK_WINDOW_X + $BLACK_WINDOW_WIDTH
	Local $endScanY = $BLACK_WINDOW_Y + 10

	$unique_colors_detected = 0

	While $endScanY <= $BLACK_WINDOW_Y + $BLACK_WINDOW_HEIGHT
		Local $aCoord = PixelSearch($startScanX, $startScanY, $endScanX, $endScanY, 0x3d200d)
		If $aCoord <> 0 And _
				_PixelGetColor_GetPixel($vDC, $aCoord[0], $aCoord[1] + 1, $hDll) == "523622" And _
				_PixelGetColor_GetPixel($vDC, $aCoord[0], $aCoord[1] + 2, $hDll) == "7B4F2C" And _
				_PixelGetColor_GetPixel($vDC, $aCoord[0] + 8, $aCoord[1] + 21, $hDll) <> "000000" Then
			$unique_colors_detected = 1
			ConsoleWrite(@CRLF & $aCoord[0] & "x" & $aCoord[1] & ": IT'S A MATCH !!! ")
			ExitLoop
		Else
			If $aCoord <> 0 Then
				Local $temp[2] = [$aCoord[0], $aCoord[1]]
				ConsoleWrite(@CRLF & $aCoord[0] & "x" & $aCoord[1] & ": NO MATCH")
				$startScanX = $aCoord[0] + 1
			Else
				$startScanX = $CHAT_WINDOW_RECTANGLE[2]
				$startScanY = $endScanY
				$endScanY = $endScanY + 1
			EndIf

			$unique_colors_detected = 0
		EndIf
	WEnd

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	Local $diff = TimerDiff($time)

	If $unique_colors_detected == 1 Then

		Local $PosX = $aCoord[0] + 5
		Local $posY = $aCoord[1] + 18

		;************** Rectangle ******************
		Local $TopLeft_X = $PosX
		Local $TopLeft_Y = $posY

		Local $BotRight_X = $TopLeft_X + 31
		Local $BotRight_Y = $TopLeft_Y + 31

		Local $Rectangle[4] = [$TopLeft_X, $TopLeft_Y, $BotRight_X, $BotRight_Y]

		$COUNTER_RECTANGLE = $Rectangle

		GUICtrlSetData($status, "Successfull")

		If $log == 1 Then
			Local $hFileOpen = FileOpen($sFilePath, 1)
			FileWrite($hFileOpen, @CRLF & "====== Initialize CounterSlot1 ======")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |CounterSlot1.found = True")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |CounterSlot1")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |X: " & $COUNTER_RECTANGLE[0] & " - " & $COUNTER_RECTANGLE[2])
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Y: " & $COUNTER_RECTANGLE[1] & " - " & $COUNTER_RECTANGLE[3])
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
			FileWrite($hFileOpen, @CRLF)
			FileClose($hFileOpen)
		EndIf
	Else
		GUICtrlSetData($status, "Failed")

		If $log == 1 Then
			Local $hFileOpen = FileOpen($sFilePath, 1)
			FileWrite($hFileOpen, @CRLF & "====== Initialize CounterSlot1 ======")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |CounterSlot1.found = False")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |CounterSlot1")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |X: " & $COUNTER_RECTANGLE[0] & " - " & $COUNTER_RECTANGLE[2])
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Y: " & $COUNTER_RECTANGLE[1] & " - " & $COUNTER_RECTANGLE[3])
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
			FileWrite($hFileOpen, @CRLF)
			FileClose($hFileOpen)
		EndIf
	EndIf

	ConsoleWrite(@CRLF & "======Func GetBrowseFieldCounterPosition======")
	ConsoleWrite(@CRLF & "COUNTER_RECTANGLE = " & $COUNTER_RECTANGLE[0] & "x" & $COUNTER_RECTANGLE[1] & " => " & $COUNTER_RECTANGLE[2] & "x" & $COUNTER_RECTANGLE[3])
	ConsoleWrite(@CRLF & "Process Time: " & $diff)
	ConsoleWrite(@CRLF & "============================================")
EndFunc   ;==>GetBrowseFieldCounterPosition
Func GetCounterDragPosition($log)
	If Not TibiaWindowIsFocused() Then
		ActivateTibiaWindow()
	EndIf

	Local $time = TimerInit()

	Local $Rectangle[2] = [$COUNTER_RECTANGLE[0] + 15, $COUNTER_RECTANGLE[1] + 15]
	$DRAG_COUNTER_POSITION = $Rectangle

	Local $diff = TimerDiff($time)


	If $DRAG_COUNTER_POSITION[0] <> 0 And $DRAG_COUNTER_POSITION[1] <> 0 Then
		GUICtrlSetData($status, "Successfull")
		If $log == 1 Then
			Local $hFileOpen = FileOpen($sFilePath, 1)
			FileWrite($hFileOpen, @CRLF & "====== Initialize CounterDragPoint ======")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |CounterDragPoint.found = True")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |CounterDragPoint")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |ClickPoint: " & $DRAG_COUNTER_POSITION[0] & " x " & $DRAG_COUNTER_POSITION[1])
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
			FileWrite($hFileOpen, @CRLF)
			FileClose($hFileOpen)
		EndIf
	Else
		GUICtrlSetData($status, "Failed")
		If $log == 1 Then
			Local $hFileOpen = FileOpen($sFilePath, 1)
			FileWrite($hFileOpen, @CRLF & "====== Initialize CounterDragPoint ======")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |CounterDragPoint.found = False")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |CounterDragPoint")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |ClickPoint: " & $DRAG_COUNTER_POSITION[0] & " x " & $DRAG_COUNTER_POSITION[1])
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
			FileWrite($hFileOpen, @CRLF)
			FileClose($hFileOpen)
		EndIf
	EndIf

	ConsoleWrite(@CRLF & "======Func GetCounterDragPosition======")
	ConsoleWrite(@CRLF & "DRAG_COUNTER_POSITION = " & $DRAG_COUNTER_POSITION[0] & "x" & $DRAG_COUNTER_POSITION[1])
	ConsoleWrite(@CRLF & "Process Time: " & $diff)
	ConsoleWrite(@CRLF & "============================================")

EndFunc   ;==>GetCounterDragPosition
Func GetCounterCheckSum()
	Local $tempCheckSum = PixelChecksum($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1], $COUNTER_RECTANGLE[2], $COUNTER_RECTANGLE[3])
	Return $tempCheckSum
EndFunc   ;==>GetCounterCheckSum
Func SaveCurrentCounterCheckSum($log)

	Local $time = TimerInit()

	GUICtrlSetData($status, "Init CounterSlot1CheckSum...")
	$COUNTER_CHECKSUM = GetCounterCheckSum()

	Local $diff = TimerDiff($time)


	If $COUNTER_CHECKSUM <> 0 Then
		GUICtrlSetData($status, "Successful")

		If $log == 1 Then
			Local $hFileOpen = FileOpen($sFilePath, 1)
			FileWrite($hFileOpen, @CRLF & "====== Initialize CurrentCounterCheckSum ======")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |CounterSlot1.found = True")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |CounterSlot1")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |CheckSum: " & $COUNTER_CHECKSUM)
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
			FileWrite($hFileOpen, @CRLF)
			FileClose($hFileOpen)
		EndIf
	Else
		GUICtrlSetData($status, "Failed")

		If $log == 1 Then
			Local $hFileOpen = FileOpen($sFilePath, 1)
			FileWrite($hFileOpen, @CRLF & "====== Initialize CurrentCounterCheckSum ======")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |CounterSlot1.found = False")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |CounterSlot1")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |CheckSum: " & $COUNTER_CHECKSUM)
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
			FileWrite($hFileOpen, @CRLF)
			FileClose($hFileOpen)
		EndIf
	EndIf

	ConsoleWrite(@CRLF & "======Func GetLockerCheckSum======")
	ConsoleWrite(@CRLF & "LOCKER_CHECKSUM = " & $COUNTER_CHECKSUM)
	ConsoleWrite(@CRLF & "Process Time: " & $diff)
	ConsoleWrite(@CRLF & "============================================")

EndFunc   ;==>SaveCurrentCounterCheckSum

;==== Locker ====
Func GetLockerPosition($log)

	If Not TibiaWindowIsFocused() Then
		ActivateTibiaWindow()
	EndIf

	Local $time = TimerInit()

	GUICtrlSetData($status, "Init Locker...")

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $startScanX = $CHAT_WINDOW_RECTANGLE[2]
	Local $startScanY = $BLACK_WINDOW_Y
	Local $endScanX = $BLACK_WINDOW_X + $BLACK_WINDOW_WIDTH
	Local $endScanY = $BLACK_WINDOW_Y + 10

	$unique_colors_detected = 0

	While $endScanY <= $BLACK_WINDOW_Y + $BLACK_WINDOW_HEIGHT
		Local $aCoord = PixelSearch($startScanX, $startScanY, $endScanX, $endScanY, 0x6c3713)
		If $aCoord <> 0 And _
				_PixelGetColor_GetPixel($vDC, $aCoord[0] + 1, $aCoord[1], $hDll) == "693310" And _
				_PixelGetColor_GetPixel($vDC, $aCoord[0] + 2, $aCoord[1], $hDll) == "63300F" Then
			$unique_colors_detected = 1
			ConsoleWrite(@CRLF & $aCoord[0] & "x" & $aCoord[1] & ": IT'S A MATCH !!! ")
			ExitLoop
		Else
			If $aCoord <> 0 Then
				Local $temp[2] = [$aCoord[0], $aCoord[1]]
				ConsoleWrite(@CRLF & $aCoord[0] & "x" & $aCoord[1] & ": NO MATCH")
				$startScanX = $aCoord[0] + 1
			Else
				$startScanX = $CHAT_WINDOW_RECTANGLE[2]
				$startScanY = $endScanY
				$endScanY = $endScanY + 1
			EndIf

			$unique_colors_detected = 0
		EndIf
	WEnd

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	Local $diff = TimerDiff($time)

	If $unique_colors_detected == 1 Then

		Local $PosX = $aCoord[0]
		Local $posY = $aCoord[1]

		;************** Locker Rectangle ******************
		Local $LockerTopLeft_X = $PosX - 3
		Local $LockerTopLeft_Y = $posY - 4

		Local $LockerBotRight_X = $LockerTopLeft_X + 31
		Local $LockerBotRight_Y = $LockerTopLeft_Y + 31

		Local $Rectangle[4] = [$LockerTopLeft_X, $LockerTopLeft_Y, $LockerBotRight_X, $LockerBotRight_Y]
		$LOCKER_RECTANGLE = $Rectangle

		GUICtrlSetData($status, "Successful")
		If $log == 1 Then
			Local $hFileOpen = FileOpen($sFilePath, 1)
			FileWrite($hFileOpen, @CRLF & "====== Initialize LockerSlot1 ======")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |LockerSlot1.found = True")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |LockerSlot1")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |X: " & $LOCKER_RECTANGLE[0] & " - " & $LOCKER_RECTANGLE[2])
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Y: " & $LOCKER_RECTANGLE[1] & " - " & $LOCKER_RECTANGLE[3])
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
			FileWrite($hFileOpen, @CRLF)
			FileClose($hFileOpen)
		EndIf
		Return True
	Else
		GUICtrlSetData($status, "Failed")
		If $log == 1 Then
			Local $hFileOpen = FileOpen($sFilePath, 1)
			FileWrite($hFileOpen, @CRLF & "====== Initialize LockerSlot1 ======")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |LockerSlot1.found = False")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |LockerSlot1")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |X: " & $LOCKER_RECTANGLE[0] & " - " & $LOCKER_RECTANGLE[2])
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Y: " & $LOCKER_RECTANGLE[1] & " - " & $LOCKER_RECTANGLE[3])
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
			FileWrite($hFileOpen, @CRLF)
			FileClose($hFileOpen)
		EndIf
		Return False
	EndIf

	ConsoleWrite(@CRLF & "======Func GetLockerPosition======")
	ConsoleWrite(@CRLF & "LOCKER_RECTANGLE = " & $LOCKER_RECTANGLE[0] & "x" & $LOCKER_RECTANGLE[1] & " => " & $LOCKER_RECTANGLE[2] & "x" & $LOCKER_RECTANGLE[3])
	ConsoleWrite(@CRLF & "Process Time: " & $diff)
	ConsoleWrite(@CRLF & "============================================")
EndFunc   ;==>GetLockerPosition
Func GetLockerDecorationRectangle($log)
	If Not TibiaWindowIsFocused() Then
		ActivateTibiaWindow()
	EndIf

	GUICtrlSetData($status, "Init LockerDecoration")

	Local $time = TimerInit()

	Local $Rectangle[4] = [$LOCKER_RECTANGLE[0] + 37, $LOCKER_RECTANGLE[1], $LOCKER_RECTANGLE[2] + 37, $LOCKER_RECTANGLE[3]]
	$LOCKER_DECORATION_RECTANGLE = $Rectangle

	Local $diff = TimerDiff($time)


	If $LOCKER_DECORATION_RECTANGLE[2] <> 0 And $LOCKER_DECORATION_RECTANGLE[3] <> 0 Then
		GUICtrlSetData($status, "Successful")
		If $log == 1 Then
			Local $hFileOpen = FileOpen($sFilePath, 1)
			FileWrite($hFileOpen, @CRLF & "====== Initialize LockerDecorationSlot ======")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |LockerSlot2.found = True")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |LockerSlot2")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |X: " & $LOCKER_DECORATION_RECTANGLE[0] & " - " & $LOCKER_DECORATION_RECTANGLE[2])
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Y: " & $LOCKER_DECORATION_RECTANGLE[1] & " - " & $LOCKER_DECORATION_RECTANGLE[3])
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
			FileWrite($hFileOpen, @CRLF)
			FileClose($hFileOpen)
		EndIf
	Else
		GUICtrlSetData($status, "Failed")
		If $log == 1 Then
			Local $hFileOpen = FileOpen($sFilePath, 1)
			FileWrite($hFileOpen, @CRLF & "====== Initialize LockerDecorationSlot ======")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |LockerSlot2.found = True")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |LockerSlot2")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |X: " & $LOCKER_DECORATION_RECTANGLE[0] & " - " & $LOCKER_DECORATION_RECTANGLE[2])
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Y: " & $LOCKER_DECORATION_RECTANGLE[1] & " - " & $LOCKER_DECORATION_RECTANGLE[3])
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
			FileWrite($hFileOpen, @CRLF)
			FileClose($hFileOpen)
		EndIf
	EndIf

	ConsoleWrite(@CRLF & "======Func GetLockerDecorationRectangle======")
	ConsoleWrite(@CRLF & "LOCKER_DECORATION_RECTANGLE = " & $LOCKER_DECORATION_RECTANGLE[0] & "x" & $LOCKER_DECORATION_RECTANGLE[1] & " => " & $LOCKER_DECORATION_RECTANGLE[2] & "x" & $LOCKER_DECORATION_RECTANGLE[3])
	ConsoleWrite(@CRLF & "Process Time: " & $diff)
	ConsoleWrite(@CRLF & "============================================")
EndFunc   ;==>GetLockerDecorationRectangle
Func GetLockerDragPosition($log)
	If Not TibiaWindowIsFocused() Then
		ActivateTibiaWindow()
	EndIf

	GUICtrlSetData($status, "Init LockerDragPoint")

	Local $time = TimerInit()

	Local $Rectangle[2] = [$LOCKER_RECTANGLE[0] + 89, $LOCKER_RECTANGLE[1] + 15]
	$DRAG_LOCKER_POSITION = $Rectangle

	Local $diff = TimerDiff($time)

	If $DRAG_LOCKER_POSITION[0] <> 0 And $DRAG_LOCKER_POSITION[1] <> 0 Then
		GUICtrlSetData($status, "Successful")
		If $log == 1 Then
			Local $hFileOpen = FileOpen($sFilePath, 1)
			FileWrite($hFileOpen, @CRLF & "====== Initialize LockerDragPoint ======")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |LockerDragPoint.found = True")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |LockerDragPoint")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |ClickPoint: " & $DRAG_LOCKER_POSITION[0] & " x " & $DRAG_LOCKER_POSITION[1])
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
			FileWrite($hFileOpen, @CRLF)
			FileClose($hFileOpen)
		EndIf
	Else
		GUICtrlSetData($status, "Failed")
		If $log == 1 Then
			Local $hFileOpen = FileOpen($sFilePath, 1)
			FileWrite($hFileOpen, @CRLF & "====== Initialize LockerDragPoint ======")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |LockerDragPoint.found = False")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |LockerDragPoint")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |ClickPoint: " & $DRAG_LOCKER_POSITION[0] & " x " & $DRAG_LOCKER_POSITION[1])
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
			FileWrite($hFileOpen, @CRLF)
			FileClose($hFileOpen)
		EndIf
	EndIf

	ConsoleWrite(@CRLF & "======Func GetLockerDragPosition======")
	ConsoleWrite(@CRLF & "DRAG_LOCKER_POSITION = " & $DRAG_LOCKER_POSITION[0] & "x" & $DRAG_LOCKER_POSITION[1])
	ConsoleWrite(@CRLF & "Process Time: " & $diff)
	ConsoleWrite(@CRLF & "============================================")

EndFunc   ;==>GetLockerDragPosition
Func GetLockerCheckSum()
	Local $tempCheckSum = PixelChecksum(($LOCKER_RECTANGLE[0] + 74), $LOCKER_RECTANGLE[1], ($LOCKER_RECTANGLE[2] + 74), $LOCKER_RECTANGLE[3])
	Return $tempCheckSum
EndFunc   ;==>GetLockerCheckSum
Func SaveCurrentLockerCheckSum($log)

	Local $time = TimerInit()

	GUICtrlSetData($status, "Init LockerSlot3CheckSum...")
	$LOCKER_CHECKSUM = GetLockerCheckSum()

	Local $diff = TimerDiff($time)


	If $LOCKER_CHECKSUM <> 0 Then
		GUICtrlSetData($status, "Successful")

		If $log == 1 Then
			Local $hFileOpen = FileOpen($sFilePath, 1)
			FileWrite($hFileOpen, @CRLF & "====== Initialize CurrentLockerCheckSum ======")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |LockerSlot3.found = True")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |LockerSlot3")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |CheckSum: " & $LOCKER_CHECKSUM)
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
			FileWrite($hFileOpen, @CRLF)
			FileClose($hFileOpen)
		EndIf
	Else
		GUICtrlSetData($status, "Failed")

		If $log == 1 Then
			Local $hFileOpen = FileOpen($sFilePath, 1)
			FileWrite($hFileOpen, @CRLF & "====== Initialize CurrentLockerCheckSum ======")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |LockerSlot3.found = False")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |LockerSlot3")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |CheckSum: " & $LOCKER_CHECKSUM)
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
			FileWrite($hFileOpen, @CRLF)
			FileClose($hFileOpen)
		EndIf
	EndIf

	ConsoleWrite(@CRLF & "======Func GetLockerCheckSum======")
	ConsoleWrite(@CRLF & "LOCKER_CHECKSUM = " & $LOCKER_CHECKSUM)
	ConsoleWrite(@CRLF & "Process Time: " & $diff)
	ConsoleWrite(@CRLF & "============================================")
EndFunc   ;==>SaveCurrentLockerCheckSum
Func GetLockerClickToOpenPosition()

	If $BLACK_NORTH Then
		Local $tempY = $TIBIA_ACTION_WINDOW_CENTRE_Y - $TILE_HEIGHT
		$LOCKER_CLICK_POSITION[0] = $TIBIA_ACTION_WINDOW_CENTRE_X
		$LOCKER_CLICK_POSITION[1] = StringFormat("%.0f", $tempY)
	Else
		Local $tempY = $TIBIA_ACTION_WINDOW_CENTRE_Y + $TILE_HEIGHT
		$LOCKER_CLICK_POSITION[0] = $TIBIA_ACTION_WINDOW_CENTRE_X
		$LOCKER_CLICK_POSITION[1] = StringFormat("%.0f", $tempY)
	EndIf

	ConsoleWrite(@CRLF & "$LOCKER_CLICK_POSITION: " & $LOCKER_CLICK_POSITION[0] & "x" & $LOCKER_CLICK_POSITION[1])
EndFunc   ;==>GetLockerClickToOpenPosition

;==== Box17 ====
Func GetBox17Position()

	If Not TibiaWindowIsFocused() Then
		ActivateTibiaWindow()
	EndIf

	GUICtrlSetData($status, "Init Box17...")

	Local $hFileOpen = FileOpen($sFilePath, 1)
	FileWrite($hFileOpen, @CRLF & "====== Initialize Box17Rectangle ======")

	Local $time = TimerInit()

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $startScanX = $CHAT_WINDOW_RECTANGLE[2]
	Local $startScanY = $BLACK_WINDOW_Y
	Local $endScanX = $BLACK_WINDOW_X + $BLACK_WINDOW_WIDTH
	Local $endScanY = $BLACK_WINDOW_Y + 10

	$unique_colors_detected = 0

	While $endScanY <= $BLACK_WINDOW_Y + $BLACK_WINDOW_HEIGHT
		Local $aCoord = PixelSearch($startScanX, $startScanY, $endScanX, $endScanY, 0x9a8d47)
		If $aCoord <> 0 And _
				_PixelGetColor_GetPixel($vDC, $aCoord[0] + 1, $aCoord[1] + 3, $hDll) == "8E8E8E" And _
				_PixelGetColor_GetPixel($vDC, $aCoord[0] + 1, $aCoord[1] + 4, $hDll) == "8F8F8F" And _
				_PixelGetColor_GetPixel($vDC, $aCoord[0] + 2, $aCoord[1] + 4, $hDll) == "B7B7B7" And _
				_PixelGetColor_GetPixel($vDC, $aCoord[0] + 3, $aCoord[1] + 4, $hDll) == "515151" Then
			$unique_colors_detected = 1
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Box17.found = True")
			ConsoleWrite(@CRLF & $aCoord[0] & "x" & $aCoord[1] & ": IT'S A MATCH !!! ")
			ExitLoop
		Else
			If $aCoord <> 0 Then
				Local $temp[2] = [$aCoord[0], $aCoord[1]]
				ConsoleWrite(@CRLF & $aCoord[0] & "x" & $aCoord[1] & ": NO MATCH")
				$startScanX = $aCoord[0] + 1
			Else
				$startScanX = $CHAT_WINDOW_RECTANGLE[2]
				$startScanY = $endScanY
				$endScanY = $endScanY + 1
			EndIf

			$unique_colors_detected = 0
		EndIf
	WEnd

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $unique_colors_detected == 1 Then

		Local $PosX = $aCoord[0] + 7
		Local $posY = $aCoord[1] + 18

		;************** Rectangle ******************
		Local $TopLeft_X = $PosX
		Local $TopLeft_Y = $posY

		Local $BotRight_X = $TopLeft_X + 31
		Local $BotRight_Y = $TopLeft_Y + 31

		Local $Rectangle[4] = [$TopLeft_X, $TopLeft_Y, $BotRight_X, $BotRight_Y]

		$BOX17_RECTANGLE = $Rectangle

		GUICtrlSetData($status, "Successful")

		FileWrite($hFileOpen, @CRLF & _NowTime() & " |Box17Slot1")
		FileWrite($hFileOpen, @CRLF & _NowTime() & " |X: " & $BOX17_RECTANGLE[0] & " - " & $BOX17_RECTANGLE[2])
		FileWrite($hFileOpen, @CRLF & _NowTime() & " |Y: " & $BOX17_RECTANGLE[1] & " - " & $BOX17_RECTANGLE[3])
	Else
		GUICtrlSetData($status, "Failed")
		FileWrite($hFileOpen, @CRLF & _NowTime() & " |Box17.found = False")
	EndIf

	Local $diff = TimerDiff($time)

	FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
	FileWrite($hFileOpen, @CRLF)
	FileClose($hFileOpen)

	ConsoleWrite(@CRLF & "======Func GetBox17Position======")
	ConsoleWrite(@CRLF & "BOX17_RECTANGLE = " & $BOX17_RECTANGLE[0] & "x" & $BOX17_RECTANGLE[1] & " => " & $BOX17_RECTANGLE[2] & "x" & $BOX17_RECTANGLE[3])
	ConsoleWrite(@CRLF & "Process Time: " & $diff)
	ConsoleWrite(@CRLF & "============================================")
EndFunc   ;==>GetBox17Position
Func GetTempContainerDepositItemsPosition()

	If Not TibiaWindowIsFocused() Then
		ActivateTibiaWindow()
	EndIf

	If $BOX17_RECTANGLE[0] <> "" Then

		GUICtrlSetData($status, "Init Box17DragPoint...")

		Local $hFileOpen = FileOpen($sFilePath, 1)
		FileWrite($hFileOpen, @CRLF & "====== Initialize Box17DragPosition ======")

		Local $time = TimerInit()

		Local $Rectangle[2] = [$BOX17_RECTANGLE[0] + 15, $BOX17_RECTANGLE[1] + 15]
		$DEPOSIT_CONTAINER_POSITION = $Rectangle

		FileWrite($hFileOpen, @CRLF & _NowTime() & " |Box17DragPoint")
		FileWrite($hFileOpen, @CRLF & _NowTime() & " |ClickPoint: " & $DEPOSIT_CONTAINER_POSITION[0] & " x " & $DEPOSIT_CONTAINER_POSITION[1])

		Local $diff = TimerDiff($time)

		FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
		FileWrite($hFileOpen, @CRLF)
		FileClose($hFileOpen)

		ConsoleWrite(@CRLF & "======Func GetTempContainerDepositItemsPosition======")
		ConsoleWrite(@CRLF & "DEPOSIT_CONTAINER_POSITION = " & $DEPOSIT_CONTAINER_POSITION[0] & "x" & $DEPOSIT_CONTAINER_POSITION[1])
		ConsoleWrite(@CRLF & "Process Time: " & $diff)
		ConsoleWrite(@CRLF & "============================================")
	EndIf

EndFunc   ;==>GetTempContainerDepositItemsPosition
Func GetBox17CheckSum()
	Local $tempCheckSum = PixelChecksum($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1], $BOX17_RECTANGLE[2], $BOX17_RECTANGLE[3])
	Return $tempCheckSum
EndFunc   ;==>GetBox17CheckSum
Func SaveCurrentBox17CheckSum($log)

	Local $time = TimerInit()

	GUICtrlSetData($status, "Init Box17CheckSum...")
	$BOX17_CHECKSUM = GetBox17CheckSum()

	Local $diff = TimerDiff($time)


	If $BOX17_CHECKSUM <> 0 Then
		GUICtrlSetData($status, "Successful")

		If $log == 1 Then
			Local $hFileOpen = FileOpen($sFilePath, 1)
			FileWrite($hFileOpen, @CRLF & "====== Initialize CurrentBox17CheckSum ======")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Box17Slot1.found = True")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Box17Slot1")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |CheckSum: " & $BOX17_CHECKSUM)
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
			FileWrite($hFileOpen, @CRLF)
			FileClose($hFileOpen)
		EndIf
	Else
		GUICtrlSetData($status, "Failed")

		If $log == 1 Then
			Local $hFileOpen = FileOpen($sFilePath, 1)
			FileWrite($hFileOpen, @CRLF & "====== Initialize CurrentBox17CheckSum ======")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Box17Slot1.found = False")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Box17Slot1")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |CheckSum: " & $BOX17_CHECKSUM)
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
			FileWrite($hFileOpen, @CRLF)
			FileClose($hFileOpen)
		EndIf
	EndIf

	ConsoleWrite(@CRLF & "======Func GetBox17CheckSum======")
	ConsoleWrite(@CRLF & "BOX17_CHECKSUM = " & $BOX17_CHECKSUM)
	ConsoleWrite(@CRLF & "Process Time: " & $diff)
	ConsoleWrite(@CRLF & "============================================")
EndFunc   ;==>SaveCurrentBox17CheckSum
Func GetBox17NumberRectangle()

	Local $time = TimerInit()

	$BOX17_NUMBER_RECTANGLE[0] = $BOX17_RECTANGLE[0] + 9
	$BOX17_NUMBER_RECTANGLE[1] = $BOX17_RECTANGLE[1] + 22
	$BOX17_NUMBER_RECTANGLE[2] = $BOX17_RECTANGLE[2]
	$BOX17_NUMBER_RECTANGLE[3] = $BOX17_RECTANGLE[1] + 29

	Local $diff = TimerDiff($time)

	Local $hFileOpen = FileOpen($sFilePath, 1)
	FileWrite($hFileOpen, @CRLF & "====== Initialize Box17NumberRectangle ======")
	FileWrite($hFileOpen, @CRLF & _NowTime() & " |Box17NumberRectangle")
	FileWrite($hFileOpen, @CRLF & _NowTime() & " |X: " & $BOX17_NUMBER_RECTANGLE[0] & " - " & $BOX17_NUMBER_RECTANGLE[2])
	FileWrite($hFileOpen, @CRLF & _NowTime() & " |Y: " & $BOX17_NUMBER_RECTANGLE[1] & " - " & $BOX17_NUMBER_RECTANGLE[3])
	FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
	FileWrite($hFileOpen, @CRLF)
	FileClose($hFileOpen)

	ConsoleWrite(@CRLF & "======Func GetBox17NumberPosition======")
	ConsoleWrite(@CRLF & "BOX17_NUMBER_RECTANGLE = " & $BOX17_NUMBER_RECTANGLE[0] & "x" & $BOX17_NUMBER_RECTANGLE[1] & " => " & $BOX17_NUMBER_RECTANGLE[2] & "x" & $BOX17_NUMBER_RECTANGLE[3])
	ConsoleWrite(@CRLF & "Process Time: " & $diff)
	ConsoleWrite(@CRLF & "============================================")
EndFunc   ;==>GetBox17NumberRectangle

;==== Box16 ====
Func GetBox16Position()

	If Not TibiaWindowIsFocused() Then
		ActivateTibiaWindow()
	EndIf


	GUICtrlSetData($status, "Init Box16...")

	Local $hFileOpen = FileOpen($sFilePath, 1)
	FileWrite($hFileOpen, @CRLF & "====== Initialize Box16Rectangle ======")

	Local $time = TimerInit()

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $startScanX = $CHAT_WINDOW_RECTANGLE[2]
	Local $startScanY = $BLACK_WINDOW_Y
	Local $endScanX = $BLACK_WINDOW_X + $BLACK_WINDOW_WIDTH
	Local $endScanY = $BLACK_WINDOW_Y + 10

	$unique_colors_detected = 0

	While $endScanY <= $BLACK_WINDOW_Y + $BLACK_WINDOW_HEIGHT
		Local $aCoord = PixelSearch($startScanX, $startScanY, $endScanX, $endScanY, 0x9a8d47)
		If $aCoord <> 0 And _
				_PixelGetColor_GetPixel($vDC, $aCoord[0] + 1, $aCoord[1] + 3, $hDll) == "AAAAAA" And _
				_PixelGetColor_GetPixel($vDC, $aCoord[0] + 1, $aCoord[1] + 4, $hDll) == "898989" And _
				_PixelGetColor_GetPixel($vDC, $aCoord[0] + 2, $aCoord[1] + 4, $hDll) == "383838" And _
				_PixelGetColor_GetPixel($vDC, $aCoord[0] + 3, $aCoord[1] + 4, $hDll) == "585858" Then
			$unique_colors_detected = 1
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Box16.found = True")
			ConsoleWrite(@CRLF & $aCoord[0] & "x" & $aCoord[1] & ": IT'S A MATCH !!! ")
			ExitLoop
		Else
			If $aCoord <> 0 Then
				Local $temp[2] = [$aCoord[0], $aCoord[1]]
				ConsoleWrite(@CRLF & $aCoord[0] & "x" & $aCoord[1] & ": NO MATCH")
				$startScanX = $aCoord[0] + 1
			Else
				$startScanX = $CHAT_WINDOW_RECTANGLE[2]
				$startScanY = $endScanY
				$endScanY = $endScanY + 1
			EndIf

			$unique_colors_detected = 0
		EndIf
	WEnd

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $unique_colors_detected == 1 Then

		Local $PosX = $aCoord[0] + 7
		Local $posY = $aCoord[1] + 18

		;************** Rectangle ******************
		Local $TopLeft_X = $PosX
		Local $TopLeft_Y = $posY

		Local $BotRight_X = $TopLeft_X + 31
		Local $BotRight_Y = $TopLeft_Y + 31

		Local $Rectangle[4] = [$TopLeft_X, $TopLeft_Y, $BotRight_X, $BotRight_Y]

		$BOX16_RECTANGLE = $Rectangle

		GUICtrlSetData($status, "Successful")
		FileWrite($hFileOpen, @CRLF & _NowTime() & " |Box16Slot1")
		FileWrite($hFileOpen, @CRLF & _NowTime() & " |X: " & $BOX16_RECTANGLE[0] & " - " & $BOX16_RECTANGLE[2])
		FileWrite($hFileOpen, @CRLF & _NowTime() & " |Y: " & $BOX16_RECTANGLE[1] & " - " & $BOX16_RECTANGLE[3])
	Else
		GUICtrlSetData($status, "Failed")
		FileWrite($hFileOpen, @CRLF & _NowTime() & " |Counter.found = False")
	EndIf


	Local $diff = TimerDiff($time)

	FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
	FileWrite($hFileOpen, @CRLF)
	FileClose($hFileOpen)
	ConsoleWrite(@CRLF & "======Func GetBox16Position======")
	ConsoleWrite(@CRLF & "BOX16_RECTANGLE = " & $BOX16_RECTANGLE[0] & "x" & $BOX16_RECTANGLE[1] & " => " & $BOX16_RECTANGLE[2] & "x" & $BOX16_RECTANGLE[3])
	ConsoleWrite(@CRLF & "Process Time: " & $diff)
	ConsoleWrite(@CRLF & "============================================")
EndFunc   ;==>GetBox16Position
Func GetBox16DragPosition()
	If Not TibiaWindowIsFocused() Then
		ActivateTibiaWindow()
	EndIf
	If $BOX16_RECTANGLE[0] <> "" Then

		GUICtrlSetData($status, "Init Box16DragPoint...")

		Local $hFileOpen = FileOpen($sFilePath, 1)
		FileWrite($hFileOpen, @CRLF & "====== Initialize Box16DragPosition ======")

		Local $time = TimerInit()

		Local $Rectangle[2] = [$BOX16_RECTANGLE[0] + 15, $BOX16_RECTANGLE[1] + 15]
		$DRAG_BOX16_POSITION = $Rectangle
		$BOX16_CHECKSUM = PixelChecksum($BOX16_RECTANGLE[0], $BOX16_RECTANGLE[1], $BOX16_RECTANGLE[2], $BOX16_RECTANGLE[3])

		FileWrite($hFileOpen, @CRLF & _NowTime() & " |Box16DragPoint")
		FileWrite($hFileOpen, @CRLF & _NowTime() & " |ClickPoint: " & $DRAG_BOX16_POSITION[0] & " x " & $DRAG_BOX16_POSITION[1])

		Local $diff = TimerDiff($time)

		FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
		FileWrite($hFileOpen, @CRLF)
		FileClose($hFileOpen)
		ConsoleWrite(@CRLF & "======Func GetBox16DragPosition======")
		ConsoleWrite(@CRLF & "DRAG_BOX16_POSITION = " & $DRAG_BOX16_POSITION[0] & "x" & $DRAG_BOX16_POSITION[1])
		ConsoleWrite(@CRLF & "Process Time: " & $diff)
		ConsoleWrite(@CRLF & "============================================")
	EndIf
EndFunc   ;==>GetBox16DragPosition
;==== Box15 ====
Func GetBox15Position()

	If Not TibiaWindowIsFocused() Then
		ActivateTibiaWindow()
	EndIf


	GUICtrlSetData($status, "Init Box15...")

	Local $hFileOpen = FileOpen($sFilePath, 1)
	FileWrite($hFileOpen, @CRLF & "====== Initialize Box15Rectangle ======")

	Local $time = TimerInit()

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $startScanX = $CHAT_WINDOW_RECTANGLE[2]
	Local $startScanY = $BLACK_WINDOW_Y
	Local $endScanX = $BLACK_WINDOW_X + $BLACK_WINDOW_WIDTH
	Local $endScanY = $BLACK_WINDOW_Y + 10

	$unique_colors_detected = 0

	While $endScanY <= $BLACK_WINDOW_Y + $BLACK_WINDOW_HEIGHT
		Local $aCoord = PixelSearch($startScanX, $startScanY, $endScanX, $endScanY, 0x9a8d47)
		If $aCoord <> 0 And _
				_PixelGetColor_GetPixel($vDC, $aCoord[0] + 1, $aCoord[1] + 3, $hDll) == "A4A4A4" And _
				_PixelGetColor_GetPixel($vDC, $aCoord[0] + 1, $aCoord[1] + 4, $hDll) == "646464" And _
				_PixelGetColor_GetPixel($vDC, $aCoord[0] + 2, $aCoord[1] + 4, $hDll) == "B7B7B7" And _
				_PixelGetColor_GetPixel($vDC, $aCoord[0] + 3, $aCoord[1] + 4, $hDll) == "959595" Then
			$unique_colors_detected = 1
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Box15.found = True")
			ConsoleWrite(@CRLF & $aCoord[0] & "x" & $aCoord[1] & ": IT'S A MATCH !!! ")
			ExitLoop
		Else
			If $aCoord <> 0 Then
				Local $temp[2] = [$aCoord[0], $aCoord[1]]
				ConsoleWrite(@CRLF & $aCoord[0] & "x" & $aCoord[1] & ": NO MATCH")
				$startScanX = $aCoord[0] + 1
			Else
				$startScanX = $CHAT_WINDOW_RECTANGLE[2]
				$startScanY = $endScanY
				$endScanY = $endScanY + 1
			EndIf

			$unique_colors_detected = 0
		EndIf
	WEnd

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $unique_colors_detected == 1 Then

		Local $PosX = $aCoord[0] + 7
		Local $posY = $aCoord[1] + 18

		;************** Rectangle ******************
		Local $TopLeft_X = $PosX
		Local $TopLeft_Y = $posY

		Local $BotRight_X = $TopLeft_X + 31
		Local $BotRight_Y = $TopLeft_Y + 31

		Local $Rectangle[4] = [$TopLeft_X, $TopLeft_Y, $BotRight_X, $BotRight_Y]

		$BOX15_RECTANGLE = $Rectangle

		GUICtrlSetData($status, "Successful")
		FileWrite($hFileOpen, @CRLF & _NowTime() & " |Box15Slot1")
		FileWrite($hFileOpen, @CRLF & _NowTime() & " |X: " & $BOX15_RECTANGLE[0] & " - " & $BOX15_RECTANGLE[2])
		FileWrite($hFileOpen, @CRLF & _NowTime() & " |Y: " & $BOX15_RECTANGLE[1] & " - " & $BOX15_RECTANGLE[3])
	Else
		GUICtrlSetData($status, "Failed")
		FileWrite($hFileOpen, @CRLF & _NowTime() & " |Box15.found = False")
	EndIf

	Local $diff = TimerDiff($time)

	FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
	FileWrite($hFileOpen, @CRLF)
	FileClose($hFileOpen)
	ConsoleWrite(@CRLF & "======Func GetBox15Position======")
	ConsoleWrite(@CRLF & "BOX15_RECTANGLE = " & $BOX15_RECTANGLE[0] & "x" & $BOX15_RECTANGLE[1] & " => " & $BOX15_RECTANGLE[2] & "x" & $BOX15_RECTANGLE[3])
	ConsoleWrite(@CRLF & "Process Time: " & $diff)
	ConsoleWrite(@CRLF & "============================================")
EndFunc   ;==>GetBox15Position
Func GetBox15DragPosition()
	If Not TibiaWindowIsFocused() Then
		ActivateTibiaWindow()
	EndIf
	If $BOX15_RECTANGLE[0] <> "" Then

		GUICtrlSetData($status, "Init Box15DragPoint...")

		Local $hFileOpen = FileOpen($sFilePath, 1)
		FileWrite($hFileOpen, @CRLF & "====== Initialize Box15DragPosition ======")

		Local $time = TimerInit()

		Local $Rectangle[2] = [$BOX15_RECTANGLE[0] + 15, $BOX15_RECTANGLE[1] + 15]
		$DRAG_BOX15_POSITION = $Rectangle
		$BOX15_CHECKSUM = PixelChecksum($BOX15_RECTANGLE[0], $BOX15_RECTANGLE[1], $BOX15_RECTANGLE[2], $BOX15_RECTANGLE[3])

		FileWrite($hFileOpen, @CRLF & _NowTime() & " |Box15DragPoint")
		FileWrite($hFileOpen, @CRLF & _NowTime() & " |ClickPoint: " & $DRAG_BOX15_POSITION[0] & " x " & $DRAG_BOX15_POSITION[1])

		Local $diff = TimerDiff($time)

		FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
		FileWrite($hFileOpen, @CRLF)
		FileClose($hFileOpen)
		ConsoleWrite(@CRLF & "======Func GetBox15DragPosition======")
		ConsoleWrite(@CRLF & "DRAG_BOX15_POSITION = " & $DRAG_BOX15_POSITION[0] & "x" & $DRAG_BOX15_POSITION[1])
		ConsoleWrite(@CRLF & "Process Time: " & $diff)
		ConsoleWrite(@CRLF & "============================================")
	EndIf
EndFunc   ;==>GetBox15DragPosition
;==== Box4 ====
Func GetBox4Position()

	If Not TibiaWindowIsFocused() Then
		ActivateTibiaWindow()
	EndIf

	GUICtrlSetData($status, "Init Box4...")

	Local $hFileOpen = FileOpen($sFilePath, 1)
	FileWrite($hFileOpen, @CRLF & "====== Initialize Box4Rectangle ======")

	Local $time = TimerInit()

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $startScanX = $CHAT_WINDOW_RECTANGLE[2]
	Local $startScanY = $BLACK_WINDOW_Y
	Local $endScanX = $BLACK_WINDOW_X + $BLACK_WINDOW_WIDTH
	Local $endScanY = $BLACK_WINDOW_Y + 10

	$unique_colors_detected = 0

	While $endScanY <= $BLACK_WINDOW_Y + $BLACK_WINDOW_HEIGHT
		Local $aCoord = PixelSearch($startScanX, $startScanY, $endScanX, $endScanY, 0x9a8d47)
		If $aCoord <> 0 And _
				_PixelGetColor_GetPixel($vDC, $aCoord[0] + 1, $aCoord[1] + 3, $hDll) == "717171" And _
				_PixelGetColor_GetPixel($vDC, $aCoord[0] + 1, $aCoord[1] + 4, $hDll) == "616161" And _
				_PixelGetColor_GetPixel($vDC, $aCoord[0] + 2, $aCoord[1] + 4, $hDll) == "2F2F2F" And _
				_PixelGetColor_GetPixel($vDC, $aCoord[0] + 3, $aCoord[1] + 4, $hDll) == "979797" Then
			$unique_colors_detected = 1
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Box4.found = True")
			ConsoleWrite(@CRLF & $aCoord[0] & "x" & $aCoord[1] & ": IT'S A MATCH !!! ")
			ExitLoop
		Else
			If $aCoord <> 0 Then
				Local $temp[2] = [$aCoord[0], $aCoord[1]]
				ConsoleWrite(@CRLF & $aCoord[0] & "x" & $aCoord[1] & ": NO MATCH")
				$startScanX = $aCoord[0] + 1
			Else
				$startScanX = $CHAT_WINDOW_RECTANGLE[2]
				$startScanY = $endScanY
				$endScanY = $endScanY + 1
			EndIf

			$unique_colors_detected = 0
		EndIf
	WEnd

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $unique_colors_detected == 1 Then

		Local $PosX = $aCoord[0] + 7
		Local $posY = $aCoord[1] + 18

		;************** Rectangle ******************
		Local $TopLeft_X = $PosX
		Local $TopLeft_Y = $posY

		Local $BotRight_X = $TopLeft_X + 31
		Local $BotRight_Y = $TopLeft_Y + 31

		Local $Rectangle[4] = [$TopLeft_X, $TopLeft_Y, $BotRight_X, $BotRight_Y]
		$BOX4_RECTANGLE = $Rectangle

		GUICtrlSetData($status, "Successful")
		FileWrite($hFileOpen, @CRLF & _NowTime() & " |Box4Slot1")
		FileWrite($hFileOpen, @CRLF & _NowTime() & " |X: " & $BOX4_RECTANGLE[0] & " - " & $BOX4_RECTANGLE[2])
		FileWrite($hFileOpen, @CRLF & _NowTime() & " |Y: " & $BOX4_RECTANGLE[1] & " - " & $BOX4_RECTANGLE[3])
	Else
		GUICtrlSetData($status, "Failed")
		FileWrite($hFileOpen, @CRLF & _NowTime() & " |Box4.found = False")
	EndIf

	Local $diff = TimerDiff($time)

	FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
	FileWrite($hFileOpen, @CRLF)
	FileClose($hFileOpen)
	ConsoleWrite(@CRLF & "======Func GetBox4Position======")
	ConsoleWrite(@CRLF & "BOX4_RECTANGLE = " & $BOX4_RECTANGLE[0] & "x" & $BOX4_RECTANGLE[1] & " => " & $BOX4_RECTANGLE[2] & "x" & $BOX4_RECTANGLE[3])
	ConsoleWrite(@CRLF & "Process Time: " & $diff)
	ConsoleWrite(@CRLF & "============================================")
EndFunc   ;==>GetBox4Position
Func GetLockerEffectItemPosition()

	If Not TibiaWindowIsFocused() Then
		ActivateTibiaWindow()
	EndIf

	If $BOX4_RECTANGLE[0] <> "" Then

		GUICtrlSetData($status, "Init LockerEffectItem...")

		Local $hFileOpen = FileOpen($sFilePath, 1)
		FileWrite($hFileOpen, @CRLF & "====== Initialize LockerEffectItemSlotRectangle ======")

		Local $time = TimerInit()

		Local $hDll = DllOpen("gdi32.dll")
		Local $vDC = _PixelGetColor_CreateDC($hDll)
		Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

		Local $startScanX = $BOX4_RECTANGLE[0]
		Local $startScanY = $BOX4_RECTANGLE[1]
		Local $endScanX = $BOX4_RECTANGLE[2] + 111
		Local $endScanY = $BOX4_RECTANGLE[3]

		$unique_colors_detected = 0

		While $endScanY <= $BLACK_WINDOW_Y + $BLACK_WINDOW_HEIGHT
			Local $aCoord = PixelSearch($startScanX, $startScanY, $endScanX, $endScanY, 0x980000)
			If $aCoord <> 0 And _
					_PixelGetColor_GetPixel($vDC, $aCoord[0] - 1, $aCoord[1], $hDll) == "352E2C" And _
					_PixelGetColor_GetPixel($vDC, $aCoord[0] + 1, $aCoord[1], $hDll) == "352E2C" And _
					_PixelGetColor_GetPixel($vDC, $aCoord[0], $aCoord[1] + 1, $hDll) == "D70000" And _
					_PixelGetColor_GetPixel($vDC, $aCoord[0], $aCoord[1] + 2, $hDll) == "980000" Then
				$unique_colors_detected = 1
				FileWrite($hFileOpen, @CRLF & _NowTime() & " |LockerEffectItem.found = True")
				ConsoleWrite(@CRLF & $aCoord[0] & "x" & $aCoord[1] & ": IT'S A MATCH !!! ")
				ExitLoop
			Else
				If $aCoord <> 0 Then
					Local $temp[2] = [$aCoord[0], $aCoord[1]]
					ConsoleWrite(@CRLF & $aCoord[0] & "x" & $aCoord[1] & ": NO MATCH")
					$startScanX = $aCoord[0] + 1
				Else
					$startScanX = $BOX4_RECTANGLE[0]
					$startScanY = $endScanY
					$endScanY = $endScanY + 1
				EndIf

				$unique_colors_detected = 0
			EndIf
		WEnd

		_PixelGetColor_ReleaseRegion($vRegion)
		_PixelGetColor_ReleaseDC($vDC, $hDll)
		DllClose($hDll)

		If $unique_colors_detected == 1 Then

			Local $PosX = $aCoord[0] + 11
			Local $posY = $aCoord[1] + 2

			;************** Rectangle ******************
			Local $TopLeft_X = $PosX
			Local $TopLeft_Y = $posY

			Local $Rectangle[2] = [$TopLeft_X, $TopLeft_Y]
			$VOODOOSKULL_POSITION = $Rectangle

			FileWrite($hFileOpen, @CRLF & _NowTime() & " |LockerEffectItemSlot")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |ClickPoint: " & $VOODOOSKULL_POSITION[0] & " x " & $VOODOOSKULL_POSITION[1])
		Else
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |LockerEffectItem.found = False")
		EndIf

		Local $diff = TimerDiff($time)

		FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
		FileWrite($hFileOpen, @CRLF)
		FileClose($hFileOpen)
		ConsoleWrite(@CRLF & "======Func GetLockerEffectItemPosition======")
		ConsoleWrite(@CRLF & "VOODOOSKULL_POSITION = " & $VOODOOSKULL_POSITION[0] & "x" & $VOODOOSKULL_POSITION[1])
		ConsoleWrite(@CRLF & "Process Time: " & $diff)
		ConsoleWrite(@CRLF & "============================================")
	EndIf

EndFunc   ;==>GetLockerEffectItemPosition
Func GetWinEffectItemPosition()

	If Not TibiaWindowIsFocused() Then
		ActivateTibiaWindow()
	EndIf

	If $BOX4_RECTANGLE[0] <> "" Then

		GUICtrlSetData($status, "Init WinEffectItem...")

		Local $hFileOpen = FileOpen($sFilePath, 1)
		FileWrite($hFileOpen, @CRLF & "====== Initialize WinEffectItemSlotRectangle ======")

		Local $time = TimerInit()

		Local $hDll = DllOpen("gdi32.dll")
		Local $vDC = _PixelGetColor_CreateDC($hDll)
		Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

		Local $startScanX = $BOX4_RECTANGLE[0]
		Local $startScanY = $BOX4_RECTANGLE[1]
		Local $endScanX = $BOX4_RECTANGLE[2] + 111
		Local $endScanY = $BOX4_RECTANGLE[3]

		$unique_colors_detected = 0

		While $endScanY <= $BLACK_WINDOW_Y + $BLACK_WINDOW_HEIGHT
			Local $aCoord = PixelSearch($startScanX, $startScanY, $endScanX, $endScanY, 0x281a0d)
			If $aCoord <> 0 And _
					_PixelGetColor_GetPixel($vDC, $aCoord[0] + 1, $aCoord[1], $hDll) == "F3CA7C" And _
					_PixelGetColor_GetPixel($vDC, $aCoord[0] + 2, $aCoord[1], $hDll) == "D59628" And _
					_PixelGetColor_GetPixel($vDC, $aCoord[0], $aCoord[1] + 1, $hDll) == "372A17" Then
				$unique_colors_detected = 1
				FileWrite($hFileOpen, @CRLF & _NowTime() & " |WinEffectItem.found = True")
				ConsoleWrite(@CRLF & $aCoord[0] & "x" & $aCoord[1] & ": IT'S A MATCH !!! ")
				ExitLoop
			Else
				If $aCoord <> 0 Then
					Local $temp[2] = [$aCoord[0], $aCoord[1]]
					ConsoleWrite(@CRLF & $aCoord[0] & "x" & $aCoord[1] & ": NO MATCH")
					$startScanX = $aCoord[0] + 1
				Else
					$startScanX = $BOX4_RECTANGLE[0]
					$startScanY = $endScanY
					$endScanY = $endScanY + 1
				EndIf

				$unique_colors_detected = 0
			EndIf
		WEnd

		_PixelGetColor_ReleaseRegion($vRegion)
		_PixelGetColor_ReleaseDC($vDC, $hDll)
		DllClose($hDll)

		If $unique_colors_detected == 1 Then

			Local $PosX = $aCoord[0] + 10
			Local $posY = $aCoord[1] + 8

			;************** Rectangle ******************
			Local $TopLeft_X = $PosX
			Local $TopLeft_Y = $posY

			Local $Rectangle[2] = [$TopLeft_X, $TopLeft_Y]
			$WIN_ANIMATION_POSITION = $Rectangle

			FileWrite($hFileOpen, @CRLF & _NowTime() & " |WinEffectItemSlot")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |ClickPoint: " & $WIN_ANIMATION_POSITION[0] & " x " & $WIN_ANIMATION_POSITION[1])
		Else
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |WinEffectItem.found = False")
		EndIf

		Local $diff = TimerDiff($time)

		FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
		FileWrite($hFileOpen, @CRLF)
		FileClose($hFileOpen)
		ConsoleWrite(@CRLF & "======Func GetWinEffectItemPosition======")
		ConsoleWrite(@CRLF & "LYRE_POSITION = " & $WIN_ANIMATION_POSITION[0] & "x" & $WIN_ANIMATION_POSITION[1])
		ConsoleWrite(@CRLF & "Process Time: " & $diff)
		ConsoleWrite(@CRLF & "============================================")
	EndIf

EndFunc   ;==>GetWinEffectItemPosition
Func GetLoseEffectItemPosition()

	If Not TibiaWindowIsFocused() Then
		ActivateTibiaWindow()
	EndIf

	If $BOX4_RECTANGLE[0] <> "" Then

		GUICtrlSetData($status, "Init LoseEffectItem...")

		Local $hFileOpen = FileOpen($sFilePath, 1)
		FileWrite($hFileOpen, @CRLF & "====== Initialize LoseEffectItemSlotRectangle ======")

		Local $time = TimerInit()

		Local $hDll = DllOpen("gdi32.dll")
		Local $vDC = _PixelGetColor_CreateDC($hDll)
		Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

		Local $startScanX = $BOX4_RECTANGLE[0]
		Local $startScanY = $BOX4_RECTANGLE[1]
		Local $endScanX = $BOX4_RECTANGLE[2] + 111
		Local $endScanY = $BOX4_RECTANGLE[3]

		$unique_colors_detected = 0

		While $endScanY <= $BLACK_WINDOW_Y + $BLACK_WINDOW_HEIGHT
			Local $aCoord = PixelSearch($startScanX, $startScanY, $endScanX, $endScanY, 0x826413)
			If $aCoord <> 0 And _
					_PixelGetColor_GetPixel($vDC, $aCoord[0] + 1, $aCoord[1], $hDll) == "856714" And _
					_PixelGetColor_GetPixel($vDC, $aCoord[0] + 2, $aCoord[1], $hDll) == "BC921E" And _
					_PixelGetColor_GetPixel($vDC, $aCoord[0], $aCoord[1] + 1, $hDll) == "000000" Then
				$unique_colors_detected = 1
				FileWrite($hFileOpen, @CRLF & _NowTime() & " |LoseEffectItem.found = True")
				ConsoleWrite(@CRLF & $aCoord[0] & "x" & $aCoord[1] & ": IT'S A MATCH !!! ")
				ExitLoop
			Else
				If $aCoord <> 0 Then
					Local $temp[2] = [$aCoord[0], $aCoord[1]]
					ConsoleWrite(@CRLF & $aCoord[0] & "x" & $aCoord[1] & ": NO MATCH")
					$startScanX = $aCoord[0] + 1
				Else
					$startScanX = $BOX4_RECTANGLE[0]
					$startScanY = $endScanY
					$endScanY = $endScanY + 1
				EndIf

				$unique_colors_detected = 0
			EndIf
		WEnd

		_PixelGetColor_ReleaseRegion($vRegion)
		_PixelGetColor_ReleaseDC($vDC, $hDll)
		DllClose($hDll)

		If $unique_colors_detected == 1 Then

			Local $PosX = $aCoord[0] + 6
			Local $posY = $aCoord[1] - 4

			;************** Rectangle ******************
			Local $TopLeft_X = $PosX
			Local $TopLeft_Y = $posY

			Local $Rectangle[2] = [$TopLeft_X, $TopLeft_Y]
			$LOSE_ANIMATION_POSITION = $Rectangle

			FileWrite($hFileOpen, @CRLF & _NowTime() & " |LoseEffectItemSlot")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |ClickPoint: " & $LOSE_ANIMATION_POSITION[0] & " x " & $LOSE_ANIMATION_POSITION[1])
		Else
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |LoseEffectItem.found = False")
		EndIf

		Local $diff = TimerDiff($time)

		FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
		FileWrite($hFileOpen, @CRLF)
		FileClose($hFileOpen)
		ConsoleWrite(@CRLF & "======Func GetLoseEffectItemPosition======")
		ConsoleWrite(@CRLF & "WARDRUM_POSITION = " & $LOSE_ANIMATION_POSITION[0] & "x" & $LOSE_ANIMATION_POSITION[1])
		ConsoleWrite(@CRLF & "Process Time: " & $diff)
		ConsoleWrite(@CRLF & "============================================")
	EndIf

EndFunc   ;==>GetLoseEffectItemPosition

;==== STORE ====
Func GetStoreRectangle()
	Local $startScanX = $BLACK_WINDOW_X + $BLACK_WINDOW_WIDTH - 181
	Local $startScanY = $BLACK_WINDOW_Y
	Local $endScanX = $BLACK_WINDOW_X + $BLACK_WINDOW_WIDTH
	Local $endScanY = $BLACK_WINDOW_Y + $BLACK_WINDOW_HEIGHT - 25

	Local $unique_colors_detected = 0

	Local $aCoord = PixelSearch($startScanX, $startScanY, $endScanX, $endScanY, 0xffdba6) ;0x42415c unique blue hex color at mana bar
	If $aCoord <> 0 And PixelSearch($aCoord[0] + 1, $aCoord[1], $aCoord[0] + 1, $aCoord[1], 0xa3612e) <> 0 Then ;0x434349 2nd unique hex color 1px right from 1st unique hex color
		$unique_colors_detected = 1
	EndIf

	If $unique_colors_detected == 1 Then
		;************** Rectangle ******************
		Local $TopLeft_X = $aCoord[0] - 4
		Local $TopLeft_Y = $aCoord[1]

		Local $BotRight_X = $aCoord[0] + 101
		Local $BotRight_Y = $aCoord[1] + 19

		Local $Rectangle[4] = [$TopLeft_X, $TopLeft_Y, $BotRight_X, $BotRight_Y]
		$STORE_RECTANGLE = $Rectangle
	EndIf

	ConsoleWrite(@CRLF & "$STORE_RECTANGLE[0] " & $STORE_RECTANGLE[0] & " $STORE_RECTANGLE[1] " & $STORE_RECTANGLE[1] & " $STORE_RECTANGLE[2] " & $STORE_RECTANGLE[2] & " $STORE_RECTANGLE[3] " & $STORE_RECTANGLE[3])
EndFunc   ;==>GetStoreRectangle

;===============
Func IsEmptySlot($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "262627" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "2a2a2b" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "2b2c2b" And _
			_PixelGetColor_GetPixel($vDC, $x + 17, $y + 14, $hDll) == "292a2a" And _
			_PixelGetColor_GetPixel($vDC, $x + 18, $y + 14, $hDll) == "262627" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "272828" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "272728" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "2c2c2c" And _
			_PixelGetColor_GetPixel($vDC, $x + 17, $y + 15, $hDll) == "2d2e2e" And _
			_PixelGetColor_GetPixel($vDC, $x + 18, $y + 15, $hDll) == "202020" Then

		_PixelGetColor_ReleaseRegion($vRegion)
		_PixelGetColor_ReleaseDC($vDC, $hDll)
		DllClose($hDll)
		Return True
	Else
		_PixelGetColor_ReleaseRegion($vRegion)
		_PixelGetColor_ReleaseDC($vDC, $hDll)
		DllClose($hDll)
		Return False
	EndIf

EndFunc   ;==>IsEmptySlot





Func SetDragPointLocker()

	GUISetState(@SW_HIDE, $GUI_Main)
	ActivateTibiaWindow()

	Local $scan_action_click_hp = 1

	If TibiaWindowIsFocused() Then

		Local $savedMousePos[2] = [-1, -1]

		While $scan_action_click_hp == 1 And TibiaWindowIsFocused()

			Local $currentMousePos = MouseGetPos()

			If $currentMousePos[0] <> $savedMousePos[0] And $currentMousePos[1] <> $savedMousePos[1] Then
				Local $savedMousePos[2] = [$currentMousePos[0], $currentMousePos[1]]

				Local $pixelColor = Hex(PixelGetColor($currentMousePos[0], $currentMousePos[1], WinGetHandle("[CLASS:Qt5QWindowIcon]")), 6)

				If $currentMousePos[0] > (@DesktopWidth / 2) Then
					$tooltipPosX = $currentMousePos[0] - 200
				ElseIf $currentMousePos[0] <= (@DesktopWidth / 2) Then
					$tooltipPosX = $currentMousePos[0] + 5
				EndIf

				Local $tooltipPosY = $currentMousePos[1] + 15

				ToolTip("PLEASE SCAN YOUR DRAG LOCKER POSITION" & @CRLF & "PRESS [ENTER] WHEN YOU ARE FINISHED" & @CRLF & "[RIGHT CLICK] FOR CANCEL" & @CRLF & "Hex color: 0x" & $pixelColor & @CRLF & "Mouse Position: " _
						 & $currentMousePos[0] & "x" & $currentMousePos[1], $tooltipPosX, $tooltipPosY, "CLICK POSITION SCAN")
			EndIf

			If _IsPressed("0D") Then
				$scan_action_click_hp = 0

				Local $tempArray[3] = [$currentMousePos[0], $currentMousePos[1], $pixelColor]

				$DRAG_LOCKER_POSITION = $tempArray

			ElseIf _IsPressed("02") Then
				$scan_action_click_hp = 0
			EndIf

		WEnd
	EndIf

	ToolTip("")
	GUISetState(@SW_SHOW, $GUI_Main)
EndFunc   ;==>SetDragPointLocker
Func SetDragPointCounter()

	GUISetState(@SW_HIDE, $GUI_Main)
	ActivateTibiaWindow()

	Local $scan_action_click_hp = 1

	If TibiaWindowIsFocused() Then

		Local $savedMousePos[2] = [-1, -1]

		While $scan_action_click_hp == 1 And TibiaWindowIsFocused()

			Local $currentMousePos = MouseGetPos()

			If $currentMousePos[0] <> $savedMousePos[0] And $currentMousePos[1] <> $savedMousePos[1] Then
				Local $savedMousePos[2] = [$currentMousePos[0], $currentMousePos[1]]

				Local $pixelColor = Hex(PixelGetColor($currentMousePos[0], $currentMousePos[1], WinGetHandle("[CLASS:Qt5QWindowIcon]")), 6)

				If $currentMousePos[0] > (@DesktopWidth / 2) Then
					$tooltipPosX = $currentMousePos[0] - 200
				ElseIf $currentMousePos[0] <= (@DesktopWidth / 2) Then
					$tooltipPosX = $currentMousePos[0] + 5
				EndIf

				Local $tooltipPosY = $currentMousePos[1] + 15

				ToolTip("PLEASE SCAN YOUR DRAG COUNTER POSITION" & @CRLF & "PRESS [ENTER] WHEN YOU ARE FINISHED" & @CRLF & "[RIGHT CLICK] FOR CANCEL" & @CRLF & "Hex color: 0x" & $pixelColor & @CRLF & "Mouse Position: " _
						 & $currentMousePos[0] & "x" & $currentMousePos[1], $tooltipPosX, $tooltipPosY, "CLICK POSITION SCAN")
			EndIf

			If _IsPressed("0D") Then
				$scan_action_click_hp = 0

				Local $tempArray[3] = [$currentMousePos[0], $currentMousePos[1], $pixelColor]

				$DRAG_COUNTER_POSITION = $tempArray

			ElseIf _IsPressed("02") Then
				$scan_action_click_hp = 0
			EndIf

		WEnd
	EndIf

	ToolTip("")
	GUISetState(@SW_SHOW, $GUI_Main)
EndFunc   ;==>SetDragPointCounter
Func SetVoodooSkullPosition()

	GUISetState(@SW_HIDE, $GUI_Main)
	ActivateTibiaWindow()

	Local $scan_action_click_hp = 1

	If TibiaWindowIsFocused() Then



		Local $savedMousePos[2] = [-1, -1]

		While $scan_action_click_hp == 1 And TibiaWindowIsFocused()

			Local $currentMousePos = MouseGetPos()

			If $currentMousePos[0] <> $savedMousePos[0] And $currentMousePos[1] <> $savedMousePos[1] Then
				Local $savedMousePos[2] = [$currentMousePos[0], $currentMousePos[1]]

				Local $pixelColor = Hex(PixelGetColor($currentMousePos[0], $currentMousePos[1], WinGetHandle("[CLASS:Qt5QWindowIcon]")), 6)

				If $currentMousePos[0] > (@DesktopWidth / 2) Then
					$tooltipPosX = $currentMousePos[0] - 200
				ElseIf $currentMousePos[0] <= (@DesktopWidth / 2) Then
					$tooltipPosX = $currentMousePos[0] + 5
				EndIf

				Local $tooltipPosY = $currentMousePos[1] + 15

				ToolTip("PLEASE SCAN YOUR VOODOOSKULL POSITION SPOT" & @CRLF & "PRESS [ENTER] WHEN YOU ARE FINISHED" & @CRLF & "[RIGHT CLICK] FOR CANCEL" & @CRLF & "Hex color: 0x" & $pixelColor & @CRLF & "Mouse Position: " _
						 & $currentMousePos[0] & "x" & $currentMousePos[1], $tooltipPosX, $tooltipPosY, "CLICK POSITION SCAN")
			EndIf

			If _IsPressed("0D") Then
				$scan_action_click_hp = 0

				Local $tempArray[3] = [$currentMousePos[0], $currentMousePos[1], $pixelColor]

				$VOODOOSKULL_POSITION = $tempArray
				$USE_VOODOOSKULL = True

			ElseIf _IsPressed("02") Then
				$scan_action_click_hp = 0
			EndIf

		WEnd
	EndIf

	ToolTip("")
	GUISetState(@SW_SHOW, $GUI_Main)
EndFunc   ;==>SetVoodooSkullPosition

Func CharacterLook($direction)

	If TibiaWindowExists() And TibiaWindowIsFocused() And CharacterLogedIn() Then

		If $direction == "North" Then
;~ 		ControlSend("[CLASS:Qt5QWindowIcon]","", "","^{UP}")
			Send("^{UP}")
			CharacterLookState_Reset()
			$looking_north = 1
		ElseIf $direction == "East" Then
;~ 		ControlSend("[CLASS:Qt5QWindowIcon]","", "","^{RIGHT}")
			Send("^{RIGHT}")
			CharacterLookState_Reset()
			$looking_east = 1
		ElseIf $direction == "South" Then
;~ 		ControlSend("[CLASS:Qt5QWindowIcon]","", "","^{DOWN}")
			Send("^{DOWN}")
			CharacterLookState_Reset()
			$looking_south = 1
		ElseIf $direction == "West" Then
;~ 		ControlSend("[CLASS:Qt5QWindowIcon]","", "","^{LEFT}")
			Send("^{LEFT}")
			CharacterLookState_Reset()
			$looking_west = 1
		EndIf

	EndIf

EndFunc   ;==>CharacterLook
Func CharacterLookState_Reset()
	$looking_north = 0
	$looking_east = 0
	$looking_south = 0
	$looking_west = 0
EndFunc   ;==>CharacterLookState_Reset

;============ CASINO ESSENTIAL FUNCTIONS ===================
Func UseVoodooSkull()
	ConsoleWrite(@CRLF & "Start UseVoodooSkull()")

	If $USE_VOODOOSKULL == True Then
		If _Timer_GetIdleTime() > 100 Then

			If Not TibiaWindowIsFocused() Then
				ActivateTibiaWindow()
			ElseIf TibiaWindowIsFocused() Then
				If TimerDiff($LAST_USED_VOODOSKULL_TIME) >= $USE_VOODOOSKULL_DELAY Then
;~ 					MouseClick($MOUSE_CLICK_RIGHT, $VOODOOSKULL_POSITION[0], $VOODOOSKULL_POSITION[1], 1, 1)
					ControlClick("[CLASS:Qt5QWindowIcon]", "", "", "right", 1, $VOODOOSKULL_POSITION[0] - 8, $VOODOOSKULL_POSITION[1] - 8)
					$LAST_USED_VOODOSKULL_TIME = TimerInit()
				EndIf
			EndIf
		EndIf
	EndIf

EndFunc   ;==>UseVoodooSkull
Func CheckCounter()
	ConsoleWrite(@CRLF & "Start CheckCounter()")
	If $CHECK_COUNTER == True Then

		If TimerDiff($LAST_COUNTER_CHECK_TIME) >= $LAST_COUNTER_CHECK_DELAY Then

			If $COUNTER_CHECKSUM <> GetCounterCheckSum() And Not CheckIfCrystalCoin($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIf100PlatCoins($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfBootsOfHaste($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfDemonArmor($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfDemonHelmet($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfBlueRobe($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfMasterMindShield($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfGoldenLegs($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfGoldenArmor($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfDemonShield($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfAmuletOfLoss($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfMagicPlateArmor($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfRoyalHelmet($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfSteelBoots($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfSoftBoots($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfAlbinoPlate($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfRoyalScaleRobe($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfRoyalDrakenMail($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfOrnateChestPlate($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfPaladinArmor($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfEarthmindRaiment($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfEliteDrakenMail($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfFirebornGiantArmor($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfFiremindRaiment($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfFrostmindRaiment($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfMasterArchersArmor($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfMoltenPlate($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfEliteDrakenHelmet($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfHatOfTheMad($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfZaoanHelmet($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfYalahariMask($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfBlueLegs($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfYalahariLegPiece($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfAbyssHammer($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfArbalest($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfArcaneStaff($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfAssassinDagger($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfBananaStaff($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfBerserker($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfBladeOfCarvingMayhemRemedy($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfBladeOfCorruption($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfBlessedSceptre($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfBloodyEdge($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfCrystallineAxe($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfDeeplingAxe($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfDemonbone($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfDemonwingAxe($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfDjinnBlade($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfGlacialRod($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfGreatAxe($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfGuardianHalberd($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfHammerOfProphecy($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfHammerOfWrath($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfHavocBlade($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfHeavyMace($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfHellforgedAxe($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfHeroicAxe($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfImpaler($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfImpalerOfTheIgniter($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfMagicLongsword($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfMagicSword($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfMaimer($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfMysticBlade($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfMythrilAxe($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfNightmareBlade($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfNobleAxe($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfNorthernStar($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfOrnamentedAxe($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfOrnateCrossbow($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfOrnateMace($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfPharaoSword($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfQueensSceptre($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfRavagersAxe($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfShadowSceptre($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfShinyBlade($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
				ConsoleWrite(@CRLF & "ANTI TRASH COUNTER")
				MouseClickDrag($MOUSE_CLICK_LEFT, $DRAG_COUNTER_POSITION[0], $DRAG_COUNTER_POSITION[1], $PLAYER_LOCKER_POSITION[0], $PLAYER_LOCKER_POSITION[1], 0)
				$LAST_COUNTER_CHECK_TIME = TimerInit()
			EndIf

		EndIf


	EndIf

EndFunc   ;==>CheckCounter
Func CheckLocker()
	ConsoleWrite(@CRLF & "Start CheckLocker()")
	If $CHECK_LOCKER == True Then

		If TimerDiff($LAST_LOCKER_CHECK_TIME) >= $LAST_LOCKER_CHECK_DELAY Then
			If $LOCKER_CHECKSUM <> GetLockerCheckSum() Then
				ConsoleWrite(@CRLF & "ANTI TRASH LOCKER")
				MouseClickDrag($MOUSE_CLICK_LEFT, $DRAG_LOCKER_POSITION[0], $DRAG_LOCKER_POSITION[1], $PLAYER_LOCKER_POSITION[0], $PLAYER_LOCKER_POSITION[1], 0)
				$LAST_LOCKER_CHECK_TIME = TimerInit()
			EndIf
		EndIf

	EndIf

EndFunc   ;==>CheckLocker
Func EatSpecialItems()

	If $CHECK_COUNTER == True Then
		ConsoleWrite(@CRLF & "Start EatSpecialItems()")

		If TimerDiff($LAST_COUNTER_CHECK_TIME) >= $LAST_COUNTER_CHECK_DELAY Then

			If $COUNTER_CHECKSUM <> GetCounterCheckSum() Then
				If CheckIfAlbinoPlate($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Or CheckIfOrnateChestPlate($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
						Or CheckIfEarthmindRaiment($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Or CheckIfMoltenPlate($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
						Or CheckIfFirebornGiantArmor($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Or CheckIfFiremindRaiment($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
						Or CheckIfFrostmindRaiment($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Or CheckIfBlessedShield($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
						Or CheckIfDragonScaleHelmet($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Or CheckIfDepthLorica($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
						Or CheckIfDwarvenHelmet($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Or CheckIfDwarvenArmor($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
						Or CheckIfGoldenHelmet($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Or CheckIfDarkLordsCape($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
						Or CheckIfVisageOfTheEndDays($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Or CheckIfBallGown($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
						Or CheckIfWerewolfHelmet($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Or CheckIfAmazonArmor($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
						Or CheckIfWingedHelmet($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Or CheckIfShieldOfCorruption($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
						Or CheckIfVampireSilkSlippers($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Or CheckIfMoltenPlate($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
						Or CheckIfTreaderOfTorment($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Or CheckIfFrostmindRaiment($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
						Or CheckIfGoldenBoots($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Or CheckIfFiremindRaiment($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
						Or CheckIfDragonScaleBoots($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Or CheckIfFirebornGiantArmor($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
						Or CheckIfCrystalBoots($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Or CheckIfEarthmindRaiment($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
						Or CheckIfBunnyslippers($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Or CheckIfOrnateChestPlate($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
						Or CheckIfShrunkenHeadNecklace($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Or CheckIfAlbinoPlate($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
						Or CheckIfVelvetMantle($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Or CheckIfAmazonHelmet($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
						Or CheckIfRobeOfTheUnderworld($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Or CheckIfAncientTiara($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
						Or CheckIfRobeOfTheIceQueen($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Or CheckIfDemonLegs($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
						Or CheckIfOceanbornLeviathanArmor($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Or CheckIfDragonScaleLegs($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
						Or CheckIfGooShell($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Or CheckIfIcyCulottes($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
						Or CheckIfFuriousFrock($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Or CheckIfOrnateLegs($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
						Or CheckIfEarthbornTitanArmor($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Or CheckIfAmazonShield($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
						Or CheckIfBladeOfCarvingMayhemRemedy($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Or CheckIfDemonwingAxe($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
						Or CheckIfGreatAxe($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then

					ConsoleWrite(@CRLF & "EAT ITEM")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DRAG_COUNTER_POSITION[0], $DRAG_COUNTER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					$LAST_COUNTER_CHECK_TIME = TimerInit()
				EndIf

				If CheckIfHammerOfProphecy($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
						Or CheckIfHavocBlade($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Or CheckIfImpalerOfTheIgniter($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
						Or CheckIfMagicLongsword($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Or CheckIfMythrilAxe($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
						Or CheckIfPharaoSword($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Or CheckIfPlagueBite($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
						Or CheckIfRavagersAxe($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then

					ConsoleWrite(@CRLF & "EAT ITEM")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DRAG_COUNTER_POSITION[0], $DRAG_COUNTER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					$LAST_COUNTER_CHECK_TIME = TimerInit()
				EndIf

			EndIf
		EndIf
	EndIf
EndFunc   ;==>EatSpecialItems
Func DetectChat()

	ConsoleWrite(@CRLF & "Start DetectChat()")

	If $PLAYER_DETECTED Then
		If TimerDiff($LAST_CHECK_CHAT) > $CHECK_CHAT_DELAY Then
			If $CHAT_WINDOW_CHECKSUM <> GetChatWindowCheckSum() Then
				If $PLAYER_DETECTED Then

					If CheckIfPlayerMessage() Then
						If $BLACKJACK_ACTIVE_GAME == 0 Then
							GetCurrentMessagePosition(0)

							GetCurrentPlayerMsg()


							If StringLower($CURRENT_PLAYER_MSG) == "info" Then
								ConsoleWrite(@CRLF & "PLAY HIGH & LOW")
								ControlSend("[CLASS:Qt5QWindowIcon]", "", "", "{F1}")
;~ 								SelfSay('high/low   [ 100% ]                               odd/even  [ 100% ]                                  1,2,3,4,5,6    [ 400% ]                          blackjack    [ 100% ]             [ Min 10k | Max 2kk ]         Private Message me only.')
							ElseIf StringLower($CURRENT_PLAYER_MSG) == "help" Then
								ControlSend("[CLASS:Qt5QWindowIcon]", "", "", "{F2}")
							ElseIf StringLower($CURRENT_PLAYER_MSG) == "h" Or StringLower($CURRENT_PLAYER_MSG) == "high" Or StringLower($CURRENT_PLAYER_MSG) == "456" Then
								ConsoleWrite(@CRLF & "PLAY HIGH & LOW")
								Play("H/L", "high")
							ElseIf StringLower($CURRENT_PLAYER_MSG) == "l" Or StringLower($CURRENT_PLAYER_MSG) == "low" Or StringLower($CURRENT_PLAYER_MSG) == "123" Then
								ConsoleWrite(@CRLF & "PLAY HIGH & LOW")
								Play("H/L", "low")
							ElseIf StringLower($CURRENT_PLAYER_MSG) == "odd" Or StringLower($CURRENT_PLAYER_MSG) == "135" Then
								ConsoleWrite(@CRLF & "PLAY HIGH & LOW")
								Play("H/L", "odd")
							ElseIf StringLower($CURRENT_PLAYER_MSG) == "even" Or StringLower($CURRENT_PLAYER_MSG) == "246" Then
								ConsoleWrite(@CRLF & "PLAY HIGH & LOW")
								Play("H/L", "even")
							ElseIf StringLower($CURRENT_PLAYER_MSG) == "1" Or StringLower($CURRENT_PLAYER_MSG) == "2" Or StringLower($CURRENT_PLAYER_MSG) == "3" _
									Or StringLower($CURRENT_PLAYER_MSG) == "4" Or StringLower($CURRENT_PLAYER_MSG) == "5" Or StringLower($CURRENT_PLAYER_MSG) == "6" Then
								ConsoleWrite(@CRLF & "PLAY NUMBERS")
								Play("Numbers", $CURRENT_PLAYER_MSG)
							ElseIf StringLower($CURRENT_PLAYER_MSG) == "blackjack" Or StringLower($CURRENT_PLAYER_MSG) == "bj" Then
								Play("BlackJack", 0)
							ElseIf StringLower($CURRENT_PLAYER_MSG) == "price" Or StringLower($CURRENT_PLAYER_MSG) == "offer" Then
								Price()
							ElseIf StringLower($CURRENT_PLAYER_MSG) == "min" Or StringLower($CURRENT_PLAYER_MSG) == "max" Then
								SelfSay("[ Min 10k | Max 2kk ]")
							ElseIf StringLower($CURRENT_PLAYER_MSG) == "cmd" Then
								SelfSay("========= COMMANDS =========          high | low | odd | even | 1-6 | blackjack | balance | price | credits    ===========================")
							ElseIf StringLower($CURRENT_PLAYER_MSG) == "trade" Then
								Trade()
							ElseIf StringLower($CURRENT_PLAYER_MSG) == "price" Then
								SelfSay("Currently under construction ...")
							ElseIf StringLower($CURRENT_PLAYER_MSG) == "sell" Then
								SelfSay("Currently under construction ...")
							ElseIf StringLower($CURRENT_PLAYER_MSG) == "buy" Then
								SelfSay("Currently under construction ...")
							ElseIf StringLower($CURRENT_PLAYER_MSG) == "balance" Then
								If $CURRENT_CHARACTER_NAME == "No Name" Then
									SelfSay("=== BALANCE ===                                         " & CheckBalance() & " cc                       =============")
								Else
									SelfSay("=== BALANCE ===                                         " & CheckBalance() + 950 & " cc                       =============")
								EndIf

							ElseIf StringLower($CURRENT_PLAYER_MSG) == "credits" Then
								SelfSay("======== CREDITS ========       Developer: bots                        Last Update:    12.11.2017                               System:   zero e                                        Contact:    
;~ 				Else
;~ 					ConsoleWrite(@CRLF & $CURRENT_PLAYER_MSG)
;~ 					SelfSay("#s " & $CURRENT_CHARACTER_NAME & " said: " & $CURRENT_PLAYER_MSG)
							EndIf

							SaveCurrentChatWindowCheckSum(0)

						ElseIf $BLACKJACK_ACTIVE_GAME == 1 Then
							GetCurrentMessagePosition(0)
							GetCurrentPlayerMsg()

							If StringLower($CURRENT_PLAYER_MSG) == "roll" Then
								Local $bj_customer_total_last_temp = $bj_customer_total
								Local $roll = Random(1, 6, 1)
								$bj_customer_total = $bj_customer_total + $roll

								If $bj_customer_total > 21 Then
									PlayLoseAnimation()
									SelfSay("#s Casino: " & $bj_host_total & ". You: " & $bj_customer_total & ". You lost !")
									$BLACKJACK_ACTIVE_GAME = 0
								ElseIf $bj_customer_total > $bj_host_total And $bj_customer_total <= 21 Then
									If $BLACKJACK_SCAM_ACTIVE Then

										If (22 - $bj_customer_total_last_temp) <= 6 Then
											Local $fake_roll = Random((22 - $bj_customer_total_last_temp), 6, 1)
											$bj_customer_total = $bj_customer_total_last_temp + $fake_roll

											$BLACKJACK_ACTIVE_GAME = 0
											PlayLoseAnimation()
											SelfSay("#s Casino: " & $bj_host_total & ". You: " & $bj_customer_total & ". You lost !")
										ElseIf ($bj_host_total - $bj_customer_total_last_temp) >= 2 Then
											Local $fake_roll = Random(1, ($bj_host_total - $bj_customer_total_last_temp), 1)
											$bj_customer_total = $bj_customer_total_last_temp + $fake_roll

											SelfSay("#s You: " & $bj_customer_total)
										EndIf


									Else
										PlayWinAnimation()
										Payout("BlackJack")
										SelfSay("#s Casino: " & $bj_host_total & ". You: " & $bj_customer_total & ". You won !")
										$BLACKJACK_ACTIVE_GAME = 0
									EndIf

								ElseIf $bj_customer_total == $bj_host_total And $bj_customer_total == 21 Then
									SelfSay("#s Casino: " & $bj_host_total & ". You: " & $bj_customer_total & ". It's a draw !")

									$BLACKJACK_ACTIVE_GAME = 1
									Local $roll
									$bj_host_total = 0
									$bj_customer_total = 0

									While $bj_host_total < 18
										$roll = Random(1, 6, 1)
										$bj_host_total = $bj_host_total + $roll
									WEnd

									If $bj_host_total > 21 Then
										PlayWinAnimation()
										Payout("BlackJack")
										SelfSay("#s Casino: " & $bj_host_total & ". You won !")
										$BLACKJACK_ACTIVE_GAME = 0
									Else
										SelfSay('#s Casino: ' & $bj_host_total & '. Your turn write "roll" and when finish "stop" !')
									EndIf
								Else
									SelfSay("#s You: " & $bj_customer_total)
								EndIf

							ElseIf StringLower($CURRENT_PLAYER_MSG) == "stop" Then
								If $bj_host_total > $bj_customer_total Then
									SelfSay("#s Casino: " & $bj_host_total & ". You: " & $bj_customer_total & ". You lost !")
									$BLACKJACK_ACTIVE_GAME = 0
								ElseIf $bj_host_total < $bj_customer_total Then
									PlayWinAnimation()
									Payout("BlackJack")
									SelfSay("#s Casino: " & $bj_host_total & ". You: " & $bj_customer_total & ". You won !")
									$BLACKJACK_ACTIVE_GAME = 0
								ElseIf $bj_host_total == $bj_customer_total Then
									SelfSay("#s Casino: " & $bj_host_total & ". You: " & $bj_customer_total & ". It's a draw !")

									$BLACKJACK_ACTIVE_GAME = 1
									Local $roll
									$bj_host_total = 0
									$bj_customer_total = 0

									While $bj_host_total < 18
										$roll = Random(1, 6, 1)
										$bj_host_total = $bj_host_total + $roll
									WEnd

									If $bj_host_total > 21 Then
										PlayWinAnimation()
										Payout("BlackJack")
										SelfSay("#s Casino: " & $bj_host_total & ". You won !")
										$BLACKJACK_ACTIVE_GAME = 0
									Else
										SelfSay('#s Casino: ' & $bj_host_total & '. Your turn write "roll" and when finish "stop" !')
									EndIf

								EndIf
							Else


							EndIf

							SaveCurrentChatWindowCheckSum(0)

						EndIf
					Else
						ConsoleWrite(@CRLF & "NO PLAYER MESSAGE")
						SaveCurrentChatWindowCheckSum(0)
					EndIf

					$LAST_CHECK_CHAT = TimerInit()

				EndIf
			EndIf
		EndIf
	EndIf

EndFunc   ;==>DetectChat
Func Play($game, $call)

	PickUpBet()

	ConsoleWrite(@CRLF & "Start CheckBetRequirement()")

	If CountBet() Then

		Local $autowin
		Local $chance
		Local $roll = Random(1, 6, 1)

		If $game == "H/L" Then
			;======= CHECK AUTOWIN H/L ======
			$chance = Random(0, 1, 1)
			If $chance == 1 Then
				$autowin = True
			ElseIf $chance == 0 Then
				$autowin = False
			EndIf
			;================================
			If CheckIfScam("H/L") Then
				If StringLower($call) == "high" Then
					$roll = Random(0, UBound($LOW_NUMBER_LIST) - 1, 1)
					GameLogPlayerLost()
					PlayLoseAnimation()
					SelfSay("<" & $LOW_NUMBER_LIST[$roll] & ">")
				ElseIf StringLower($call) == "low" Then
					$roll = Random(0, UBound($HIGH_NUMBER_LIST) - 1, 1)
					GameLogPlayerLost()
					PlayLoseAnimation()
					SelfSay("<" & $HIGH_NUMBER_LIST[$roll] & ">")
				ElseIf StringLower($call) == "odd" Then
					$roll = Random(0, UBound($EVEN_NUMBER_LIST) - 1, 1)
					GameLogPlayerLost()
					PlayLoseAnimation()
					SelfSay("<" & $EVEN_NUMBER_LIST[$roll] & ">")
				ElseIf StringLower($call) == "even" Then
					$roll = Random(0, UBound($ODD_NUMBER_LIST) - 1, 1)
					GameLogPlayerLost()
					PlayLoseAnimation()
					SelfSay("<" & $ODD_NUMBER_LIST[$roll] & ">")
				EndIf
			Else
				If StringLower($call) == "high" Then
					If $autowin Then
						$roll = Random(0, UBound($HIGH_NUMBER_LIST) - 1, 1)
						PlayWinAnimation()
						Payout("H/L")
						GameLogPlayerWin()
						SelfSay("<" & $HIGH_NUMBER_LIST[$roll] & ">")
					ElseIf Not $autowin Then
						$roll = Random(0, UBound($LOW_NUMBER_LIST) - 1, 1)
						GameLogPlayerLost()
						PlayLoseAnimation()
						SelfSay("<" & $LOW_NUMBER_LIST[$roll] & ">")
					EndIf
				ElseIf StringLower($call) == "low" Then
					If $autowin Then
						$roll = Random(0, UBound($LOW_NUMBER_LIST) - 1, 1)
						PlayWinAnimation()
						Payout("H/L")
						GameLogPlayerWin()
						SelfSay("<" & $LOW_NUMBER_LIST[$roll] & ">")
					ElseIf Not $autowin Then
						$roll = Random(0, UBound($HIGH_NUMBER_LIST) - 1, 1)
						GameLogPlayerLost()
						PlayLoseAnimation()
						SelfSay("<" & $HIGH_NUMBER_LIST[$roll] & ">")
					EndIf
				ElseIf StringLower($call) == "odd" Then
					If $autowin Then
						$roll = Random(0, UBound($ODD_NUMBER_LIST) - 1, 1)
						PlayWinAnimation()
						Payout("H/L")
						GameLogPlayerWin()
						SelfSay("<" & $ODD_NUMBER_LIST[$roll] & ">")
					ElseIf Not $autowin Then
						$roll = Random(0, UBound($EVEN_NUMBER_LIST) - 1, 1)
						GameLogPlayerLost()
						PlayLoseAnimation()
						SelfSay("<" & $EVEN_NUMBER_LIST[$roll] & ">")
					EndIf
				ElseIf StringLower($call) == "even" Then
					If $autowin Then
						$roll = Random(0, UBound($EVEN_NUMBER_LIST) - 1, 1)
						PlayWinAnimation()
						Payout("H/L")
						GameLogPlayerWin()
						SelfSay("<" & $EVEN_NUMBER_LIST[$roll] & ">")
					ElseIf Not $autowin Then
						$roll = Random(0, UBound($ODD_NUMBER_LIST) - 1, 1)
						GameLogPlayerLost()
						PlayLoseAnimation()
						SelfSay("<" & $ODD_NUMBER_LIST[$roll] & ">")
					EndIf
				EndIf
			EndIf
		ElseIf $game == "Numbers" Then
			;======= CHECK AUTOWIN NUMBERS ======
			$chance = Random(0, 5, 1)
			If $chance == 1 Then
				$autowin = True
			Else
				$autowin = False
			EndIf
			;================================
			If CheckIfScam("Numbers") Then
				$roll = Random(0, UBound($FULL_NUMBER_LIST) - 1, 1)
				While $FULL_NUMBER_LIST[$roll] == $call
					$roll = Random(0, UBound($FULL_NUMBER_LIST) - 1, 1)
				WEnd
				GameLogPlayerLost()
				PlayLoseAnimation()
				SelfSay("<" & $FULL_NUMBER_LIST[$roll] & ">")
			Else
				If $autowin Then
					PlayWinAnimation()
					Payout("Numbers")
					GameLogPlayerWin()
					SelfSay("<" & $call & ">")
				ElseIf Not $autowin Then
					$roll = Random(0, UBound($FULL_NUMBER_LIST) - 1, 1)
					While $FULL_NUMBER_LIST[$roll] == $call
						$roll = Random(0, UBound($FULL_NUMBER_LIST) - 1, 1)
					WEnd
					GameLogPlayerLost()
					PlayLoseAnimation()
					SelfSay("<" & $FULL_NUMBER_LIST[$roll] & ">")
				EndIf
			EndIf
		ElseIf $game == "XO" Then
			;********* TEST MODE ************
		ElseIf $game == "BlackJack" Then

			$BLACKJACK_ACTIVE_GAME = 1
			$bj_host_total = 0
			$bj_customer_total = 0

			$BLACKJACK_SCAM_ACTIVE = False

			If CheckIfScam("BlackJack") Then
				$bj_host_total = Random(18, 21, 1)
				$BLACKJACK_SCAM_ACTIVE = True
			Else
				While $bj_host_total < 18
					$roll = Random(1, 6, 1)
					$bj_host_total = $bj_host_total + $roll
				WEnd
			EndIf




			If $bj_host_total > 21 Then
				PlayWinAnimation()
				Payout("BlackJack")
				SelfSay("#s Casino: " & $bj_host_total & ". You won !")
				$BLACKJACK_ACTIVE_GAME = 0
			Else
				SelfSay('#s Casino: ' & $bj_host_total & '. Your turn write "roll" and when finish "stop" !')
			EndIf

		EndIf

		If $game == "H/L" Or $game == "Numbers" Then
			GameLogChat($call, $roll)
		EndIf

		SetPlayerData()
		SetCasinoBalance()
	EndIf

EndFunc   ;==>Play


Func PickUpBet()

	$ITEM_DETECTED = 0

	If $COUNTER_CHECKSUM <> GetCounterCheckSum() Then

		$ITEM_DETECTED = 1

		While $COUNTER_CHECKSUM <> GetCounterCheckSum()

				If Not CheckIfCrystalCoin($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIf100PlatCoins($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfBootsOfHaste($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfDemonArmor($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfDemonHelmet($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfBlueRobe($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfMasterMindShield($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfGoldenLegs($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfGoldenArmor($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfDemonShield($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfAmuletOfLoss($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfMagicPlateArmor($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfRoyalHelmet($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfSteelBoots($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfSoftBoots($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfAlbinoPlate($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfRoyalScaleRobe($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfRoyalDrakenMail($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfOrnateChestPlate($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfPaladinArmor($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfEarthmindRaiment($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfEliteDrakenMail($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfFirebornGiantArmor($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfFiremindRaiment($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfFrostmindRaiment($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfMasterArchersArmor($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfMoltenPlate($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfEliteDrakenHelmet($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfHatOfTheMad($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfZaoanHelmet($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfYalahariMask($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfBlueLegs($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfYalahariLegPiece($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfAbyssHammer($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfArbalest($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfArcaneStaff($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfAssassinDagger($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfBananaStaff($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfBerserker($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfBladeOfCarvingMayhemRemedy($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfBladeOfCorruption($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfBlessedSceptre($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfBloodyEdge($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfCrystallineAxe($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfDeeplingAxe($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfDemonbone($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfDemonwingAxe($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfDjinnBlade($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfGlacialRod($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfGreatAxe($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfGuardianHalberd($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfHammerOfProphecy($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfHammerOfWrath($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfHavocBlade($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfHeavyMace($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfHellforgedAxe($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfHeroicAxe($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfImpaler($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfImpalerOfTheIgniter($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfMagicLongsword($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfMagicSword($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfMaimer($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfMysticBlade($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfMythrilAxe($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfNightmareBlade($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfNobleAxe($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfNorthernStar($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfOrnamentedAxe($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfOrnateCrossbow($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfOrnateMace($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfPharaoSword($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfQueensSceptre($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfRavagersAxe($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) And Not CheckIfShadowSceptre($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
					And Not CheckIfShinyBlade($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
				ConsoleWrite(@CRLF & "ANTI TRASH COUNTER")
				MouseClickDrag($MOUSE_CLICK_LEFT, $DRAG_COUNTER_POSITION[0], $DRAG_COUNTER_POSITION[1], $PLAYER_LOCKER_POSITION[0], $PLAYER_LOCKER_POSITION[1], 0)
				$LAST_COUNTER_CHECK_TIME = TimerInit()
			Else
		;~ 		If CheckIfCrystalCoin($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		;~ 			ConsoleWrite(@CRLF & "<Crystal Coin> COUNTER ==> TEMP BOX 17")
		;~ 			MouseClickDrag($MOUSE_CLICK_LEFT, $DRAG_COUNTER_POSITION[0], $DRAG_COUNTER_POSITION[1], $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], 0)
		;~ 			MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X,$TIBIA_ACTION_WINDOW_CENTRE_Y,0)
		;~ 		ElseIf CheckIfBootsOfHaste($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Or CheckIfDemonArmor($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
		;~ 			Or CheckIfDemonHelmet($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Or CheckIfBlueRobe($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
		;~ 			Or CheckIfMasterMindShield($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Or CheckIfGoldenLegs($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
		;~ 			Or CheckIfGoldenArmor($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Or CheckIfDemonShield($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
		;~ 			Or CheckIfAmuletOfLoss($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Or CheckIfMagicPlateArmor($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
		;~ 			Or CheckIfRoyalHelmet($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Or CheckIfSteelBoots($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
		;~ 			Or CheckIfSoftBoots($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Or CheckIfAlbinoPlate($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
		;~ 			Or CheckIfRoyalScaleRobe($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) 	Or CheckIfRoyalDrakenMail($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
		;~ 			Or CheckIfPaladinArmor($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Or CheckIfEarthmindRaiment($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
		;~ 			Or CheckIfEliteDrakenMail($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Or CheckIfFirebornGiantArmor($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
		;~ 			Or CheckIfFiremindRaiment($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Or CheckIfFrostmindRaiment($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
		;~ 			Or CheckIfMasterArchersArmor($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Or CheckIfMoltenPlate($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) _
		;~ 			Or CheckIfOrnateChestPlate($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then

					ConsoleWrite(@CRLF & "<Valid Item> COUNTER ==> TEMP BOX 17")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DRAG_COUNTER_POSITION[0], $DRAG_COUNTER_POSITION[1], $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
		;~ 		Else
		;~ 			ConsoleWrite(@CRLF & "<Unknown Item> COUNTER ==> PLAYER LOCKER")
		;~ 			MouseClickDrag($MOUSE_CLICK_LEFT, $DRAG_COUNTER_POSITION[0], $DRAG_COUNTER_POSITION[1], $PLAYER_LOCKER_POSITION[0], $PLAYER_LOCKER_POSITION[1], 0)
		;~ 			MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X,$TIBIA_ACTION_WINDOW_CENTRE_Y,0)
		;~ 		EndIf
			EndIf

		WEnd
	EndIf

EndFunc   ;==>PickUpBet
Func CountBet()

	$PLAYER_BET = 0

	If GetBox17CheckSum() <> $BOX17_CHECKSUM Then

		While GetBox17CheckSum() <> $BOX17_CHECKSUM

			ConsoleWrite(@CRLF & "BOX17 CHECKSUM = " & GetBox17CheckSum() & " ||| SAVED BOX17 CHECKSUM = " & $BOX17_CHECKSUM)

			If CheckIfCrystalCoin($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + CountCrystalCoins($BOX17_NUMBER_RECTANGLE[0], $BOX17_NUMBER_RECTANGLE[1])
				While CheckIfCrystalCoin($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Crystal Coin> TEMP BOX 17 ==> BOX 16")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX16_POSITION[0], $DRAG_BOX16_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIf100PlatCoins($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 1
				While CheckIf100PlatCoins($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Platinum Coins> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfBootsOfHaste($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 3
				While CheckIfBootsOfHaste($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Boots of Haste> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfDemonArmor($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 18
				While CheckIfDemonArmor($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Demon Armor> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfDemonHelmet($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 5
				While CheckIfDemonHelmet($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Demon Helmet> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfBlueRobe($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 1
				While CheckIfBlueRobe($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Blue Robe> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfMasterMindShield($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 5
				While CheckIfMasterMindShield($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Mastermind Shield> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfGoldenLegs($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 3
				While CheckIfGoldenLegs($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Golden Legs> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfGoldenArmor($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 2
				While CheckIfGoldenArmor($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Golden Armor> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfDemonShield($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 3
				While CheckIfDemonShield($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Demon Shield> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfAmuletOfLoss($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 5
				While CheckIfAmuletOfLoss($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Amulet of Loss> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfMagicPlateArmor($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 9
				While CheckIfMagicPlateArmor($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Magic Plate Armor> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfRoyalHelmet($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 3
				While CheckIfRoyalHelmet($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Royal Helmet> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfSteelBoots($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 3
				While CheckIfSteelBoots($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Steel Boots> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfAlbinoPlate($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 100
				While CheckIfAlbinoPlate($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Albino Plate> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfRoyalScaleRobe($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 77
				While CheckIfRoyalScaleRobe($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Royal Scale Robe> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfRoyalDrakenMail($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 65
				While CheckIfRoyalDrakenMail($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Royal Draken Mail> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfPaladinArmor($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 2
				While CheckIfPaladinArmor($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Paladin Armor> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfEarthmindRaiment($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 100
				While CheckIfEarthmindRaiment($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Earthmind Raiment> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfEliteDrakenMail($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 5
				While CheckIfEliteDrakenMail($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Elite Draken Mail> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfFirebornGiantArmor($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 35
				While CheckIfFirebornGiantArmor($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Fireborn Giant Armor> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfFiremindRaiment($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 100
				While CheckIfFiremindRaiment($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Firemind Raiment> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfFrostmindRaiment($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 100
				While CheckIfFrostmindRaiment($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Frostmind Raiment> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfMasterArchersArmor($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 40
				While CheckIfMasterArchersArmor($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Master Archers Armor> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfMoltenPlate($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 100
				While CheckIfMoltenPlate($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Molten Plate> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfOrnateChestPlate($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 100
				While CheckIfOrnateChestPlate($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Ornate Chestplate> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfYalahariLegPiece($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 28
				While CheckIfYalahariLegPiece($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<alahari Leg Piece> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfYalahariMask($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 25
				While CheckIfYalahariMask($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Yalahari Mask> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfHatOfTheMad($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 4
				While CheckIfHatOfTheMad($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Ornate Chestplate> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfEliteDrakenHelmet($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 90
				While CheckIfEliteDrakenHelmet($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Elite Draken Helmet> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfZaoanHelmet($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 4
				While CheckIfZaoanHelmet($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Zaoan Helmet> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfBlueLegs($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 6
				While CheckIfBlueLegs($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Blue Legs> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfAbyssHammer($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 2
				While CheckIfAbyssHammer($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Abyss Hammer> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfArbalest($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 4
				While CheckIfArbalest($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Arbalest> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfArcaneStaff($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 4
				While CheckIfArcaneStaff($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Arcane Staff> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfAssassinDagger($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 2
				While CheckIfAssassinDagger($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Assassin Dagger> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfBananaStaff($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 3
				While CheckIfBananaStaff($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Banana Staff> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfBananaStaff($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 3
				While CheckIfBananaStaff($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Banana Staff> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfBerserker($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 4
				While CheckIfBerserker($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Berserker> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfBladeOfCorruption($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 5
				While CheckIfBladeOfCorruption($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Blade Of Corruption> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfBlessedSceptre($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 4
				While CheckIfBlessedSceptre($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Blessed Sceptre> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfBloodyEdge($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 3
				While CheckIfBloodyEdge($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Bloody Edge> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfCrystallineAxe($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 15
				While CheckIfCrystallineAxe($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Crystalline Axe> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfDeeplingAxe($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 4
				While CheckIfDeeplingAxe($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Deepling Axe> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfDemonbone($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 49
				While CheckIfDemonbone($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Demonbone> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfDjinnBlade($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 41
				While CheckIfDjinnBlade($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Djinn Blade> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfGlacialRod($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 9
				While CheckIfGlacialRod($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Glacial Rod> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfGuardianHalberd($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 10
				While CheckIfGuardianHalberd($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Guardian Halberd> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfHammerOfWrath($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 3
				While CheckIfHammerOfWrath($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Hammer Of Wrath> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfHeavyMace($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 5
				While CheckIfHeavyMace($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Heavy Mace> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfHellforgedAxe($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 15
				While CheckIfHellforgedAxe($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Hellforged Axe> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfHeroicAxe($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 15
				While CheckIfHeroicAxe($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Heroic Axe> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfImpaler($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 25
				While CheckIfImpaler($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Impaler> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfMagicSword($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 20
				While CheckIfMagicSword($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Magic Sword> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfMaimer($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 10
				While CheckIfMaimer($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Maimer> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfMysticBlade($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 3
				While CheckIfMysticBlade($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Mystic Blade> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfNightmareBlade($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 3
				While CheckIfNightmareBlade($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Nightmare Blade> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfNobleAxe($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 1
				While CheckIfNobleAxe($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Noble Axe> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfNorthernStar($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 50
				While CheckIfNorthernStar($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Northern Star> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfOrnamentedAxe($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 10
				While CheckIfOrnamentedAxe($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Ornamented Axe> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfOrnateCrossbow($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 1
				While CheckIfOrnateCrossbow($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Ornate Crossbow> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfOrnateMace($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 4
				While CheckIfOrnateMace($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Ornate Mace> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfQueensSceptre($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 15
				While CheckIfQueensSceptre($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Queen's Sceptre> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfShadowSceptre($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 1
				While CheckIfShadowSceptre($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Shadow Sceptre> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			ElseIf CheckIfShinyBlade($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1]) Then
				$PLAYER_BET = $PLAYER_BET + 60
				While CheckIfShinyBlade($BOX17_RECTANGLE[0], $BOX17_RECTANGLE[1])
					ConsoleWrite(@CRLF & "<Shiny Blade> TEMP BOX 17 ==> BOX 15")
					MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $DRAG_BOX15_POSITION[0], $DRAG_BOX15_POSITION[1], 0)
					MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
				WEnd
			Else
				ConsoleWrite(@CRLF & "<Invalid Item> TEMP BOX 17 ==> PLAYER LOCKER")
				MouseClickDrag($MOUSE_CLICK_LEFT, $DEPOSIT_CONTAINER_POSITION[0], $DEPOSIT_CONTAINER_POSITION[1], $PLAYER_LOCKER_POSITION[0], $PLAYER_LOCKER_POSITION[1], 0)
				MouseMove($TIBIA_ACTION_WINDOW_CENTRE_X, $TIBIA_ACTION_WINDOW_CENTRE_Y, 0)
			EndIf

			Sleep(750)
		WEnd
		Return True
	Else
		If $ITEM_DETECTED == 1 Then
			ConsoleWrite(@CRLF & "No Bet on Counter!")
			SelfSay("Don't try to cheat me my friend!")
		ElseIf $ITEM_DETECTED == 0 Then
			ConsoleWrite(@CRLF & "No Bet on Counter!")
			SelfSay("Sorry but min bet is 10k !")
		EndIf

		Return False

	EndIf

EndFunc   ;==>CountBet
Func Price()
	If CheckIfCrystalCoin($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		Local $tempCC = CountCrystalCoins($COUNTER_RECTANGLE[0] + 9, $COUNTER_RECTANGLE[1] + 22)
		SelfSay($tempCC & " Crystal Coins => " & $tempCC * 10 & " k")

	ElseIf CheckIf100PlatCoins($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("100 Platinum Coins => 10k")

	ElseIf CheckIfBootsOfHaste($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Boots of Haste => 30k")

	ElseIf CheckIfDemonArmor($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Demon Armor => 180k")

	ElseIf CheckIfDemonHelmet($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Demon Helmet => 50k")

	ElseIf CheckIfBlueRobe($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Blue Robe => 10k")

	ElseIf CheckIfMasterMindShield($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Mastermind Shield => 50k")

	ElseIf CheckIfGoldenLegs($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Golden Legs => 30k")

	ElseIf CheckIfGoldenArmor($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Golden Armor => 20k")

	ElseIf CheckIfDemonShield($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Demon Shield => 30k")

	ElseIf CheckIfAmuletOfLoss($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Amulet of Loss => 50k")

	ElseIf CheckIfMagicPlateArmor($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Magic Plate Armor => 90k")

	ElseIf CheckIfRoyalHelmet($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Royal Helmet => 30k")

	ElseIf CheckIfSteelBoots($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Steel Boots => 30k")

	ElseIf CheckIfAlbinoPlate($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Albino Plate => 1kk")

	ElseIf CheckIfRoyalScaleRobe($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Royal Scale Robe => 770k")

	ElseIf CheckIfRoyalDrakenMail($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Royal Draken Mail => 750k")

	ElseIf CheckIfPaladinArmor($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Paladin Armor => 20k")

	ElseIf CheckIfEarthmindRaiment($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Earthmind Raiment => 5kk")

	ElseIf CheckIfEliteDrakenMail($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Elite Draken Mail => 50k")

	ElseIf CheckIfFirebornGiantArmor($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Fireborn Giant Armor => 350k")

	ElseIf CheckIfFiremindRaiment($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Firemind Raiment => 5kk")

	ElseIf CheckIfFrostmindRaiment($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Frostmind Raiment => 5kk")

	ElseIf CheckIfMasterArchersArmor($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Masterarchers Armor => 500k")

	ElseIf CheckIfOrnateChestPlate($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Ornate Chestplate => 6kk")

	ElseIf CheckIfYalahariLegPiece($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Yalahari Leg Piece => 280k")

	ElseIf CheckIfYalahariMask($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Yalahari Mask => 250k")

	ElseIf CheckIfHatOfTheMad($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Hat of the Mad => 40k")

	ElseIf CheckIfEliteDrakenHelmet($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Elite Draken Helmet => 900k")

	ElseIf CheckIfZaoanHelmet($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Zaoan Helmet => 40k")

	ElseIf CheckIfBlueLegs($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Blue Legs => 60k")

	ElseIf CheckIfAbyssHammer($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Abyss Hammer => 20k")

	ElseIf CheckIfArbalest($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Arbalest => 40k")

	ElseIf CheckIfArcaneStaff($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Arcane Staff => 40k")

	ElseIf CheckIfAssassinDagger($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Assasin Dagger => 20k")

	ElseIf CheckIfBananaStaff($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Banana Staff => 60k")

	ElseIf CheckIfBerserker($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Berserker => 40k")

	ElseIf CheckIfBladeOfCorruption($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Blade of Corruption => 50k")

	ElseIf CheckIfBlessedSceptre($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Blessed Sceptre => 40k")

	ElseIf CheckIfBloodyEdge($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Bloody Edge => 30k")

	ElseIf CheckIfCrystallineAxe($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Crystalline Axe => 150k")

	ElseIf CheckIfDeeplingAxe($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Deepling Axe => 40k")

	ElseIf CheckIfDemonbone($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Demonbone => 490k")

	ElseIf CheckIfDjinnBlade($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Djinn Blade => 410k")

	ElseIf CheckIfGlacialRod($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Glacial Rod => 90k")

	ElseIf CheckIfGuardianHalberd($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Guardian Halberd => 10k")

	ElseIf CheckIfHammerOfWrath($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Hammer of Wrath => 30k")

	ElseIf CheckIfHeavyMace($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Heavy Mace => 50k")

	ElseIf CheckIfHellforgedAxe($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Hellfordged Axe => 150k")

	ElseIf CheckIfHeroicAxe($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Heroic Axe => 20k")

	ElseIf CheckIfImpaler($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Impaler => 250k")

	ElseIf CheckIfMagicSword($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Magic Sword => 200k")

	ElseIf CheckIfMaimer($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Maimer => 100k")

	ElseIf CheckIfMysticBlade($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Mystic Blade => 30k")

	ElseIf CheckIfNightmareBlade($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Nightmare Blade => 30k")

	ElseIf CheckIfNobleAxe($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Noble Axe => 10k")

	ElseIf CheckIfNorthernStar($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Northern Star => 500k")

	ElseIf CheckIfOrnamentedAxe($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Ornamented Axe => 100k")

	ElseIf CheckIfOrnateCrossbow($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Ornate Crossbow => 10k")

	ElseIf CheckIfOrnateMace($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Ornate Mace => 40k")

	ElseIf CheckIfQueensSceptre($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Queens Sceptre => 150k")

	ElseIf CheckIfShadowSceptre($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Shadow Sceptre => 10k")

	ElseIf CheckIfShinyBlade($COUNTER_RECTANGLE[0], $COUNTER_RECTANGLE[1]) Then
		SelfSay("Shiny Blade => 600k")

	Else
		SelfSay("Sorry but the first item is not in my list")
	EndIf
EndFunc   ;==>Price

Func Payout($game)
	ConsoleWrite(@CRLF & "Start Payout()")

	Local $current_cc_slot1
	Local $tempBalance
	Local $winrate
	Local $pay = 0

	If $game == "H/L" Then
		$winrate = ($HLPayout + 100) / 100
	ElseIf $game == "Numbers" Then
		$winrate = ($NRPayout + 100) / 100
	ElseIf $game == "XO" Then
		$winrate = ($XOPayout + 100) / 100
	ElseIf $game == "BlackJack" Then
		$winrate = ($BJPayout + 100) / 100
	EndIf

	$pay = $PLAYER_BET * $winrate

	ConsoleWrite(@CRLF & "============Payout Info=============")
	ConsoleWrite(@CRLF & "PLAYER BET: " & $PLAYER_BET)
	ConsoleWrite(@CRLF & "WINRATE " & $winrate)
	ConsoleWrite(@CRLF & "PAYOUT: " & $pay)
	ConsoleWrite(@CRLF & "====================================")

;~ While $pay > 0
;~ While CheckIfCrystalCoin($BOX16_RECTANGLE[0], $BOX16_RECTANGLE[1]) == True
;~ ConsoleWrite(@CRLF & "<Crystal Coin> BOX 16 ==> PLAYER LOCKER")
;~ 	Send("{CTRLDOWN}")
;~ 	MouseClickDrag($MOUSE_CLICK_LEFT, $DRAG_BOX16_POSITION[0], $DRAG_BOX16_POSITION[1], $PLAYER_LOCKER_POSITION[0], $PLAYER_LOCKER_POSITION[1], 5)
;~ 	Send("{CTRLUP}")
;~ 	Sleep(500)

;~ 	ControlSend("Tibia - dominik","", "","{2}")
;~ While $pay > 0



	While $pay > 0

		$current_cc_slot1 = CountCrystalCoins($BOX16_RECTANGLE[0] + 9, $BOX16_RECTANGLE[1] + 22)


		If $current_cc_slot1 >= $pay Then
			$tempBalance = CheckBalance() - $pay

			Sleep(250)
			Send("{CTRLDOWN}")
			MouseClickDrag($MOUSE_CLICK_LEFT, $DRAG_BOX16_POSITION[0], $DRAG_BOX16_POSITION[1], $PLAYER_LOCKER_POSITION[0], $PLAYER_LOCKER_POSITION[1], 0)
			Send("{CTRLUP}")
			Sleep(250)
			SendKeyNumber($pay)
			Sleep(250)
;~ 			MouseClick($MOUSE_CLICK_LEFT, $MOVE_OBJECT_OK_POSITION[0], $MOVE_OBJECT_OK_POSITION[1], 1, 0)
			Send("{ENTER}")
			Sleep(1000)

			While CheckBalance() <> $tempBalance
				Sleep(250)
				Send("{CTRLDOWN}")
				MouseClickDrag($MOUSE_CLICK_LEFT, $DRAG_BOX16_POSITION[0], $DRAG_BOX16_POSITION[1], $PLAYER_LOCKER_POSITION[0], $PLAYER_LOCKER_POSITION[1], 0)
				Send("{CTRLUP}")
				Sleep(250)
				SendKeyNumber($pay)
				Sleep(250)
;~ 			MouseClick($MOUSE_CLICK_LEFT, $MOVE_OBJECT_OK_POSITION[0], $MOVE_OBJECT_OK_POSITION[1], 1, 0)
				Send("{ENTER}")
				Sleep(1000)
			WEnd
			ConsoleWrite(@CRLF & "<" & $pay & " cc> BOX16 ==> PLAYER LOCKER")
			$pay = $pay - $pay
		ElseIf $current_cc_slot1 < $pay Then
			$tempBalance = CheckBalance() - $current_cc_slot1

			Sleep(250)
			Send("{CTRLDOWN}")
			MouseClickDrag($MOUSE_CLICK_LEFT, $DRAG_BOX16_POSITION[0], $DRAG_BOX16_POSITION[1], $PLAYER_LOCKER_POSITION[0], $PLAYER_LOCKER_POSITION[1], 0)
			Send("{CTRLUP}")
			Sleep(250)
;~ 			MouseClick($MOUSE_CLICK_LEFT, $MOVE_OBJECT_OK_POSITION[0], $MOVE_OBJECT_OK_POSITION[1], 1, 0)
			Send("{ENTER}")
			Sleep(1000)

			While CheckBalance() <> $tempBalance
				Sleep(250)
				Send("{CTRLDOWN}")
				MouseClickDrag($MOUSE_CLICK_LEFT, $DRAG_BOX16_POSITION[0], $DRAG_BOX16_POSITION[1], $PLAYER_LOCKER_POSITION[0], $PLAYER_LOCKER_POSITION[1], 0)
				Send("{CTRLUP}")
				Sleep(250)
;~ 			MouseClick($MOUSE_CLICK_LEFT, $MOVE_OBJECT_OK_POSITION[0], $MOVE_OBJECT_OK_POSITION[1], 1, 0)
				Send("{ENTER}")
				Sleep(1000)
			WEnd

			ConsoleWrite(@CRLF & "<" & $current_cc_slot1 & " cc> BOX16 ==> PLAYER LOCKER")
			$pay = $pay - $current_cc_slot1
		EndIf

	WEnd
EndFunc   ;==>Payout
Func DetectPlayer()

	Local $empty_switch = 0
	Local $current_checksum = GetPlayerSwitchCheckSum()

	For $x = 0 To (UBound($CHECKSUMS) - 1)
		If $current_checksum == $CHECKSUMS[$x] Then
			$empty_switch = 1
		EndIf
	Next

	If $empty_switch == 0 Then
		If TimerDiff($PLAYER_SWITCH_CHECK_TIME) > $PLAYER_SWITCH_CHECK_DELAY Then
			If Not $PLAYER_DETECTED Then
				If CheckIfPlayer() Then
					If $BLACK_WEST Then
						CharacterLook("East")
					ElseIf $BLACK_EAST Then
						CharacterLook("West")
					EndIf
					RightClickPlayer()
					If GetCharacterNameRectangle() == False Then
						ConsoleWrite(@CRLF & "GetCharNameRectangle <FAILED>")
;~ 							MouseClick($MOUSE_CLICK_LEFT, $PLAYER_CLICK_POSITION[0], $PLAYER_CLICK_POSITION[1], 1, 0)
						Local $aCoord = PixelSearch($PLAYER_CLICK_POSITION[0], $PLAYER_CLICK_POSITION[1], $PLAYER_CLICK_POSITION[0] + 1, $PLAYER_CLICK_POSITION[1] + 350, 0x171717)
						While $aCoord == 1
							$aCoord = PixelSearch($PLAYER_CLICK_POSITION[0], $PLAYER_CLICK_POSITION[1], $PLAYER_CLICK_POSITION[0] + 1, $PLAYER_CLICK_POSITION[1] + 350, 0x171717)
							ControlClick("[CLASS:Qt5QWindowIcon]", "", "", "left", 1, $PLAYER_CLICK_POSITION[0] - 8, $PLAYER_CLICK_POSITION[1] - 8)
							Sleep(250)
						WEnd

						Return False
					Else
						ConsoleWrite(@CRLF & "GetCharNameRectangle <FAILED>")
						Local $aCoord = PixelSearch($PLAYER_CLICK_POSITION[0], $PLAYER_CLICK_POSITION[1], $PLAYER_CLICK_POSITION[0] + 1, $PLAYER_CLICK_POSITION[1] + 350, 0x171717)
						While $aCoord == 1
							$aCoord = PixelSearch($PLAYER_CLICK_POSITION[0], $PLAYER_CLICK_POSITION[1], $PLAYER_CLICK_POSITION[0] + 1, $PLAYER_CLICK_POSITION[1] + 350, 0x171717)
							OpenChatChannel()
							Sleep(250)
						WEnd
					EndIf
					GetCurrentUsername()
					PlayerListAdd()
					SetPlayerData()
					OpenChatChannel()
					If $BLACKJACK_ACTIVE_GAME == 1 Then
						SelfSay('#s Welcome to my casino ' & $CURRENT_CHARACTER_NAME & ' ! BlackJack is still active please write "roll" or "stop"')
					Else
						SelfSay('Welcome to my casino ' & $CURRENT_CHARACTER_NAME & ' ! Message me in private chat and write "info". !! How to play write "help" !!')
					EndIf
					GUICtrlSetData($black_player, $CURRENT_CHARACTER_NAME)
					$PLAYER_DETECTED = True
				Else
					SaveCurrentPlayerCheckSum(0)
				EndIf
			ElseIf $PLAYER_DETECTED And Not CheckIfPlayer() Then
;~ 				SaveCurrentPlayerCheckSum(0)
				If $BLACK_NORTH Then
					If $looking_south <> 1 Then
						CharacterLook("South")
						CloseCurrentChatChannel()
						ResetPlayerData()
						$PLAYER_DETECTED = False
					EndIf
				ElseIf $BLACK_SOUTH Then
					If $looking_north <> 1 Then
						CharacterLook("North")
						CloseCurrentChatChannel()
						ResetPlayerData()
						$PLAYER_DETECTED = False
					EndIf
				EndIf
				$PLAYER_SWITCH_CHECK_TIME = TimerInit()
			ElseIf $PLAYER_DETECTED And CheckIfPlayer() Then
				If $BLACK_WEST Then
					If $looking_east <> 1 Then
						CharacterLook("East")
						GUICtrlSetData($black_player, $CURRENT_CHARACTER_NAME)
					EndIf
				ElseIf $BLACK_EAST Then
					If $looking_west <> 1 Then
						CharacterLook("West")
						GUICtrlSetData($black_player, $CURRENT_CHARACTER_NAME)
					EndIf
				EndIf
			EndIf
		EndIf
	ElseIf $empty_switch == 1 Then
		If $PLAYER_DETECTED Then
			If Not CheckIfPlayer() Then
				If $BLACK_NORTH Then
					CharacterLook("South")
				ElseIf $BLACK_SOUTH Then
					CharacterLook("North")
				EndIf
				CloseCurrentChatChannel()
				ResetPlayerData()
				$PLAYER_DETECTED = False
				$PLAYER_SWITCH_CHECK_TIME = TimerInit()
			EndIf
		Else

			If $BLACK_NORTH Then
				If $looking_south <> 1 Then
					CharacterLook("South")
					CloseCurrentChatChannel()
					ResetPlayerData()
					$PLAYER_DETECTED = False
					$PLAYER_SWITCH_CHECK_TIME = TimerInit()
				EndIf
			ElseIf $BLACK_SOUTH Then
				If $looking_north <> 1 Then
					CharacterLook("North")
					CloseCurrentChatChannel()
					ResetPlayerData()
					$PLAYER_DETECTED = False
					$PLAYER_SWITCH_CHECK_TIME = TimerInit()
				EndIf
			EndIf
		EndIf
	EndIf

EndFunc   ;==>DetectPlayer
Func CheckIfPlayer()

	If PixelSearch($PLAYER_TILE_RECTANGLE[0], $PLAYER_TILE_RECTANGLE[1] - 30, $PLAYER_TILE_RECTANGLE[2], $PLAYER_TILE_RECTANGLE[1], 0x00c000) <> 0 Then
		Return True
	ElseIf PixelSearch($PLAYER_TILE_RECTANGLE[0], $PLAYER_TILE_RECTANGLE[1] - 30, $PLAYER_TILE_RECTANGLE[2], $PLAYER_TILE_RECTANGLE[1], 0x60c060) <> 0 Then
		Return True
	ElseIf PixelSearch($PLAYER_TILE_RECTANGLE[0], $PLAYER_TILE_RECTANGLE[1] - 30, $PLAYER_TILE_RECTANGLE[2], $PLAYER_TILE_RECTANGLE[1], 0xc0c000) <> 0 Then
		Return True
	ElseIf PixelSearch($PLAYER_TILE_RECTANGLE[0], $PLAYER_TILE_RECTANGLE[1] - 30, $PLAYER_TILE_RECTANGLE[2], $PLAYER_TILE_RECTANGLE[1], 0xc03030) <> 0 Then
		Return True
	ElseIf PixelSearch($PLAYER_TILE_RECTANGLE[0], $PLAYER_TILE_RECTANGLE[1] - 30, $PLAYER_TILE_RECTANGLE[2], $PLAYER_TILE_RECTANGLE[1], 0xc00000) <> 0 Then
		Return True
	ElseIf PixelSearch($PLAYER_TILE_RECTANGLE[0], $PLAYER_TILE_RECTANGLE[1] - 30, $PLAYER_TILE_RECTANGLE[2], $PLAYER_TILE_RECTANGLE[1], 0x600000) <> 0 Then
		Return True
	EndIf

	Return False

;~ 00c000 full
;~ 60c060 light green
;~ c0c000 yellow
;~ c03030 light red
;~ c00000 dark red
;~ 600000 very dark red

EndFunc   ;==>CheckIfPlayer
Func CountCrystalCoins($startX, $startY)

	ConsoleWrite(@CRLF & "Start  CountCrystalCoins(" & $startX & "," & $startY & ")")

	Local $NumbersStartBlock1_X = $startX
	Local $NumbersStartBlock1_Y = $startY

	Local $NumbersStartBlock2_X = $startX + 8
	Local $NumbersStartBlock2_Y = $startY

	Local $NumbersStartBlock3_X = $startX + 16
	Local $NumbersStartBlock3_Y = $startY



	Local $Number = ""

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

;~ 	For $x=0 To 24
;~ 		For $y=0 To 8
;~ 			GUICtrlCreateLabel("",25+$x,100+$y,1,1)
;~ 			GUICtrlSetBkColor(-1,"0x" & _PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X+$x ,$NumbersStartBlock1_Y+$y, $hDll))
;~ 		Next
;~ 	Next


	If Not CheckIfCrystalCoin($startX - 9, $startY - 22) Then
		$Number = 0
	Else

		;NUMBER BLOCK 1
		If _PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 1, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 3, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 5, $hDll) == "BFBFBF" Then
			$Number = $Number & "0"
		ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X + 2, $NumbersStartBlock1_Y + 6, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X + 3, $NumbersStartBlock1_Y + 7, $hDll) == "BFBFBF" Then
			$Number = $Number & "1"
		ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 1, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 2, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 3, $hDll) <> "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 4, $hDll) <> "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 5, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 6, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 7, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 8, $hDll) == "000000" Then
			$Number = $Number & "2"
		ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X + 1, $NumbersStartBlock1_Y + 3, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X + 2, $NumbersStartBlock1_Y + 3, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X + 5, $NumbersStartBlock1_Y + 2, $hDll) == "BFBFBF" Then
			$Number = $Number & "3"
		ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y, $hDll) <> "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 1, $hDll) <> "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 2, $hDll) <> "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 3, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 4, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 5, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 6, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 7, $hDll) <> "BFBFBF" Then
			$Number = $Number & "4"
		ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 1, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 2, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 3, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 4, $hDll) <> "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 5, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 6, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 7, $hDll) == "000000" Then
			$Number = $Number & "5"
		ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y, $hDll) <> "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 1, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 2, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 3, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 4, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 5, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 6, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 7, $hDll) == "000000" Then
			$Number = $Number & "6"
		ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 1, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 2, $hDll) <> "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 3, $hDll) <> "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 4, $hDll) <> "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 5, $hDll) <> "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 6, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 7, $hDll) == "000000" Then
			$Number = $Number & "7"
		ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 1, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 2, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 3, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 4, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 5, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 6, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 7, $hDll) == "000000" Then
			$Number = $Number & "8"
		ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 1, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 2, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 3, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 4, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 5, $hDll) <> "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 6, $hDll) <> "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 7, $hDll) == "000000" Then
			$Number = $Number & "9"
		Else
			$Number = $Number & ""
		EndIf

		;NUMBER BLOCK 2
		If _PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 1, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 3, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 5, $hDll) == "BFBFBF" Then
			$Number = $Number & "0"
		ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X + 2, $NumbersStartBlock2_Y + 6, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X + 3, $NumbersStartBlock2_Y + 7, $hDll) == "BFBFBF" Then
			$Number = $Number & "1"
		ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 1, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 2, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 3, $hDll) <> "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 4, $hDll) <> "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 5, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 6, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 7, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 8, $hDll) == "000000" Then
			$Number = $Number & "2"
		ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X + 1, $NumbersStartBlock2_Y + 3, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X + 2, $NumbersStartBlock2_Y + 3, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X + 5, $NumbersStartBlock2_Y + 2, $hDll) == "BFBFBF" Then
			$Number = $Number & "3"
		ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y, $hDll) <> "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 1, $hDll) <> "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 2, $hDll) <> "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 3, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 4, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 5, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 6, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 7, $hDll) <> "BFBFBF" Then
			$Number = $Number & "4"
		ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 1, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 2, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 3, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 4, $hDll) <> "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 5, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 6, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 7, $hDll) == "000000" Then
			$Number = $Number & "5"
		ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y, $hDll) <> "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 1, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 2, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 3, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 4, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 5, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 6, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 7, $hDll) == "000000" Then
			$Number = $Number & "6"
		ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 1, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 2, $hDll) <> "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 3, $hDll) <> "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 4, $hDll) <> "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 5, $hDll) <> "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 6, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 7, $hDll) == "000000" Then
			$Number = $Number & "7"
		ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 1, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 2, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 3, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 4, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 5, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 6, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 7, $hDll) == "000000" Then
			$Number = $Number & "8"
		ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 1, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 2, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 3, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 4, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 5, $hDll) <> "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 6, $hDll) <> "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 7, $hDll) == "000000" Then
			$Number = $Number & "9"
		Else
			$Number = $Number & ""
		EndIf

		;NUMBER BLOCK 3
		If _PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 1, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 3, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 5, $hDll) == "BFBFBF" Then
			$Number = $Number & "0"
		ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X + 2, $NumbersStartBlock3_Y + 6, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X + 3, $NumbersStartBlock3_Y + 7, $hDll) == "BFBFBF" Then
			$Number = $Number & "1"
		ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 1, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 2, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 3, $hDll) <> "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 4, $hDll) <> "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 5, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 6, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 7, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 8, $hDll) == "000000" Then
			$Number = $Number & "2"
		ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X + 1, $NumbersStartBlock3_Y + 3, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X + 2, $NumbersStartBlock3_Y + 3, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X + 5, $NumbersStartBlock3_Y + 2, $hDll) == "BFBFBF" Then
			$Number = $Number & "3"
		ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y, $hDll) <> "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 1, $hDll) <> "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 2, $hDll) <> "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 3, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 4, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 5, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 6, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 7, $hDll) <> "BFBFBF" Then
			$Number = $Number & "4"
		ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 1, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 2, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 3, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 4, $hDll) <> "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 5, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 6, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 7, $hDll) == "000000" Then
			$Number = $Number & "5"
		ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y, $hDll) <> "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 1, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 2, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 3, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 4, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 5, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 6, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 7, $hDll) == "000000" Then
			$Number = $Number & "6"
		ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 1, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 2, $hDll) <> "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 3, $hDll) <> "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 4, $hDll) <> "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 5, $hDll) <> "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 6, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 7, $hDll) == "000000" Then
			$Number = $Number & "7"
		ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 1, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 2, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 3, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 4, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 5, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 6, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 7, $hDll) == "000000" Then
			$Number = $Number & "8"
		ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 1, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 2, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 3, $hDll) == "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 4, $hDll) == "000000" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 5, $hDll) <> "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 6, $hDll) <> "BFBFBF" And _
				_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 7, $hDll) == "000000" Then
			$Number = $Number & "9"
		Else
			$Number = $Number & ""
		EndIf

		If $Number == "" Then
			$Number = "1"
		EndIf
	EndIf

	ConsoleWrite(@CRLF & "Counted Crystal Coins = " & $Number)

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)


	Return $Number

EndFunc   ;==>CountCrystalCoins
Func CountPlatinCoins($startX, $startY)

	ConsoleWrite(@CRLF & "Start  CountCrystalCoins(" & $startX & "," & $startY & ")")

	Local $NumbersStartBlock1_X = $startX
	Local $NumbersStartBlock1_Y = $startY

	Local $NumbersStartBlock2_X = $startX + 8
	Local $NumbersStartBlock2_Y = $startY

	Local $NumbersStartBlock3_X = $startX + 16
	Local $NumbersStartBlock3_Y = $startY



	Local $Number = ""

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

;~ 	For $x=0 To 24
;~ 		For $y=0 To 8
;~ 			GUICtrlCreateLabel("",25+$x,100+$y,1,1)
;~ 			GUICtrlSetBkColor(-1,"0x" & _PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X+$x ,$NumbersStartBlock1_Y+$y, $hDll))
;~ 		Next
;~ 	Next




	;NUMBER BLOCK 1
	If _PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 1, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 3, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 5, $hDll) == "BFBFBF" Then
		$Number = $Number & "0"
	ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X + 2, $NumbersStartBlock1_Y + 6, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X + 3, $NumbersStartBlock1_Y + 7, $hDll) == "BFBFBF" Then
		$Number = $Number & "1"
	ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 1, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 3, $hDll) <> "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 4, $hDll) <> "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 6, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 7, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 8, $hDll) == "000000" Then
		$Number = $Number & "2"
	ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X + 1, $NumbersStartBlock1_Y + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X + 2, $NumbersStartBlock1_Y + 3, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X + 5, $NumbersStartBlock1_Y + 2, $hDll) == "BFBFBF" Then
		$Number = $Number & "3"
	ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y, $hDll) <> "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 1, $hDll) <> "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 2, $hDll) <> "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 4, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 5, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 7, $hDll) <> "BFBFBF" Then
		$Number = $Number & "4"
	ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 4, $hDll) <> "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 6, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 7, $hDll) == "000000" Then
		$Number = $Number & "5"
	ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y, $hDll) <> "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 2, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 3, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 4, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 5, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 6, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 7, $hDll) == "000000" Then
		$Number = $Number & "6"
	ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 2, $hDll) <> "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 3, $hDll) <> "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 4, $hDll) <> "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 5, $hDll) <> "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 7, $hDll) == "000000" Then
		$Number = $Number & "7"
	ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 1, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 2, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 4, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 5, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 6, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 7, $hDll) == "000000" Then
		$Number = $Number & "8"
	ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 1, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 2, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 3, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 5, $hDll) <> "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 6, $hDll) <> "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock1_X, $NumbersStartBlock1_Y + 7, $hDll) == "000000" Then
		$Number = $Number & "9"
	Else
		$Number = $Number & ""
	EndIf

	;NUMBER BLOCK 2
	If _PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 1, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 3, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 5, $hDll) == "BFBFBF" Then
		$Number = $Number & "0"
	ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X + 2, $NumbersStartBlock2_Y + 6, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X + 3, $NumbersStartBlock2_Y + 7, $hDll) == "BFBFBF" Then
		$Number = $Number & "1"
	ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 1, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 3, $hDll) <> "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 4, $hDll) <> "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 6, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 7, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 8, $hDll) == "000000" Then
		$Number = $Number & "2"
	ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X + 1, $NumbersStartBlock2_Y + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X + 2, $NumbersStartBlock2_Y + 3, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X + 5, $NumbersStartBlock2_Y + 2, $hDll) == "BFBFBF" Then
		$Number = $Number & "3"
	ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y, $hDll) <> "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 1, $hDll) <> "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 2, $hDll) <> "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 4, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 5, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 7, $hDll) <> "BFBFBF" Then
		$Number = $Number & "4"
	ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 4, $hDll) <> "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 6, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 7, $hDll) == "000000" Then
		$Number = $Number & "5"
	ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y, $hDll) <> "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 2, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 3, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 4, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 5, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 6, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 7, $hDll) == "000000" Then
		$Number = $Number & "6"
	ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 2, $hDll) <> "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 3, $hDll) <> "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 4, $hDll) <> "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 5, $hDll) <> "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 7, $hDll) == "000000" Then
		$Number = $Number & "7"
	ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 1, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 2, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 4, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 5, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 6, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 7, $hDll) == "000000" Then
		$Number = $Number & "8"
	ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 1, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 2, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 3, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 5, $hDll) <> "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 6, $hDll) <> "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock2_X, $NumbersStartBlock2_Y + 7, $hDll) == "000000" Then
		$Number = $Number & "9"
	Else
		$Number = $Number & ""
	EndIf

	;NUMBER BLOCK 3
	If _PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 1, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 3, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 5, $hDll) == "BFBFBF" Then
		$Number = $Number & "0"
	ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X + 2, $NumbersStartBlock3_Y + 6, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X + 3, $NumbersStartBlock3_Y + 7, $hDll) == "BFBFBF" Then
		$Number = $Number & "1"
	ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 1, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 3, $hDll) <> "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 4, $hDll) <> "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 6, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 7, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 8, $hDll) == "000000" Then
		$Number = $Number & "2"
	ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X + 1, $NumbersStartBlock3_Y + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X + 2, $NumbersStartBlock3_Y + 3, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X + 5, $NumbersStartBlock3_Y + 2, $hDll) == "BFBFBF" Then
		$Number = $Number & "3"
	ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y, $hDll) <> "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 1, $hDll) <> "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 2, $hDll) <> "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 4, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 5, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 7, $hDll) <> "BFBFBF" Then
		$Number = $Number & "4"
	ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 4, $hDll) <> "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 6, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 7, $hDll) == "000000" Then
		$Number = $Number & "5"
	ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y, $hDll) <> "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 2, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 3, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 4, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 5, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 6, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 7, $hDll) == "000000" Then
		$Number = $Number & "6"
	ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 2, $hDll) <> "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 3, $hDll) <> "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 4, $hDll) <> "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 5, $hDll) <> "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 7, $hDll) == "000000" Then
		$Number = $Number & "7"
	ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 1, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 2, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 4, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 5, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 6, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 7, $hDll) == "000000" Then
		$Number = $Number & "8"
	ElseIf _PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 1, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 2, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 3, $hDll) == "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 5, $hDll) <> "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 6, $hDll) <> "BFBFBF" And _
			_PixelGetColor_GetPixel($vDC, $NumbersStartBlock3_X, $NumbersStartBlock3_Y + 7, $hDll) == "000000" Then
		$Number = $Number & "9"
	Else
		$Number = $Number & ""
	EndIf

	If $Number == "" Then
		$Number = "1"
	EndIf

	ConsoleWrite(@CRLF & "Counted Crystal Coins = " & $Number)

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)


	Return $Number

EndFunc   ;==>CountPlatinCoins
Func Announcement()
	If TimerDiff($LAST_ANNOUNCEMENT_TIME) >= (Random(40, 45, 0) * 1000 * 60) Then
		SelfSay("#s PLAY HIGH/LOW/ODD/EVEN/BLACKJACK 100% PAYOUT NUMBERS 400% PAYOUT - REAL DICE WIN CHANCE! TRY YOUR CHANCE NOW!")
		$LAST_ANNOUNCEMENT_TIME = TimerInit()
	EndIf
EndFunc   ;==>Announcement

Func SpecialEventAnnouncement()
	If TimerDiff($LAST_ANNOUNCEMENT_TIME) >= (Random(15, 45, 1) * 1000 * 60) Then

;~ 		SelfSay("#s   ~~WINNER WINNER CHICKEN DINNER !!! TRY YOUR LUCK NOW AND PLAY BLACKJACK !!!~~")
		ControlSend("[CLASS:Qt5QWindowIcon]", "", "", "{F3}")
;~ 		SelfSay("#s [SCAMMER]                                                    Crystal The Antica                             [SCAMMER]")
		$LAST_ANNOUNCEMENT_TIME = TimerInit()
	EndIf
EndFunc   ;==>SpecialEventAnnouncement
Func RandomJokes()

	If TimerDiff($LAST_ANNOUNCEMENT_TIME) >= (Random(10, 15, 1) * 1000 * 60) Then
		$temp = Random(0, UBound($JokeCollection)-1, 1)
		SelfSay("#s " & $JokeCollection[$temp])

		$LAST_ANNOUNCEMENT_TIME = TimerInit()
	EndIf

EndFunc   ;==>RandomJokes


Func GetCurrentPlayerMsg()
	Local $time = TimerInit()

	Local $string_result
	Local $empty_string = 0

	Local $Msg_width = $MSG_RECTANGLE[2] - $MSG_RECTANGLE[0]

	$PLAYER_MSG_STRING_BLOCK_SCAN_X = $MSG_RECTANGLE[0]
	$PLAYER_MSG_STRING_BLOCK_SCAN_Y = $MSG_RECTANGLE[1]

	$CURRENT_PLAYER_MSG = ""

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	While $PLAYER_MSG_STRING_BLOCK_SCAN_X < $MSG_RECTANGLE[0] + 120
		If CheckCurrentMessageLetter($PLAYER_MSG_STRING_BLOCK_SCAN_X, $PLAYER_MSG_STRING_BLOCK_SCAN_Y, $hDll, $vDC, "60F8F8") == False Then
			$empty_string = $empty_string + 1
			If $empty_string == 2 Then
				$CURRENT_PLAYER_MSG = StringTrimRight($CURRENT_PLAYER_MSG, 1)
				ExitLoop
			Else
				$CURRENT_PLAYER_MSG = $CURRENT_PLAYER_MSG & " "
				$PLAYER_MSG_STRING_BLOCK_SCAN_X = $PLAYER_MSG_STRING_BLOCK_SCAN_X + 4
			EndIf
		Else
			$empty_string = 0
			ConsoleWrite(@CRLF & "$PLAYER_MSG_STRING_BLOCK_SCAN_X= " & $PLAYER_MSG_STRING_BLOCK_SCAN_X)
		EndIf
	WEnd

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	ConsoleWrite(@CRLF & "$CURRENT_PLAYER_MSG = " & $CURRENT_PLAYER_MSG)

	Return $CURRENT_PLAYER_MSG
EndFunc   ;==>GetCurrentPlayerMsg
Func Trade()

	Local $tempSlot1 = ""

	If CheckIfBootsOfHaste($BOX15_RECTANGLE[0], $BOX15_RECTANGLE[1]) Then
		$tempSlot1 = "Boots of Haste"
	ElseIf CheckIfDemonArmor($BOX15_RECTANGLE[0], $BOX15_RECTANGLE[1]) Then
		$tempSlot1 = "Demon Armor"
	ElseIf CheckIfDemonHelmet($BOX15_RECTANGLE[0], $BOX15_RECTANGLE[1]) Then
		$tempSlot1 = "Demon Helmet"
	ElseIf CheckIfBlueRobe($BOX15_RECTANGLE[0], $BOX15_RECTANGLE[1]) Then
		$tempSlot1 = "Blue Robe"
	ElseIf CheckIfMasterMindShield($BOX15_RECTANGLE[0], $BOX15_RECTANGLE[1]) Then
		$tempSlot1 = "Mastermind Shield"
	ElseIf CheckIfGoldenLegs($BOX15_RECTANGLE[0], $BOX15_RECTANGLE[1]) Then
		$tempSlot1 = "Golden Legs"
	ElseIf CheckIfGoldenArmor($BOX15_RECTANGLE[0], $BOX15_RECTANGLE[1]) Then
		$tempSlot1 = "Golden Armor"
	ElseIf CheckIfDemonShield($BOX15_RECTANGLE[0], $BOX15_RECTANGLE[1]) Then
		$tempSlot1 = "Demon Shield"
	ElseIf CheckIfAmuletOfLoss($BOX15_RECTANGLE[0], $BOX15_RECTANGLE[1]) Then
		$tempSlot1 = "Amulet of Loss"
	ElseIf CheckIfMagicPlateArmor($BOX15_RECTANGLE[0], $BOX15_RECTANGLE[1]) Then
		$tempSlot1 = "Magic Plate Armor"
	ElseIf CheckIfRoyalHelmet($BOX15_RECTANGLE[0], $BOX15_RECTANGLE[1]) Then
		$tempSlot1 = "Royal Helmet"
	ElseIf CheckIfSteelBoots($BOX15_RECTANGLE[0], $BOX15_RECTANGLE[1]) Then
		$tempSlot1 = "Steel Boots"
	ElseIf CheckIfSoftBoots($BOX15_RECTANGLE[0], $BOX15_RECTANGLE[1]) Then
		$tempSlot1 = "Pair of Soft Boots"
	Else
		$tempSlot1 = ""
	EndIf

	If $tempSlot1 <> "" Then
		SelfSay('I can sell "' & $tempSlot1 & '" for 30k.')
	Else
		SelfSay('Sorry but currently I have nothing for sell. Ask me later again.')
	EndIf

EndFunc   ;==>Trade

Func CheckBalance()
	Local $tempBalance = 0

	$tempCalc = CountCrystalCoins($BOX16_RECTANGLE[0] + 9, $BOX16_RECTANGLE[1] + 22)
	$tempBalance = $tempBalance + $tempCalc
	$tempCalc = CountCrystalCoins($BOX16_RECTANGLE[0] + 46, $BOX16_RECTANGLE[1] + 22)
	$tempBalance = $tempBalance + $tempCalc
	$tempCalc = CountCrystalCoins($BOX16_RECTANGLE[0] + 83, $BOX16_RECTANGLE[1] + 22)
	$tempBalance = $tempBalance + $tempCalc
	$tempCalc = CountCrystalCoins($BOX16_RECTANGLE[0] + 120, $BOX16_RECTANGLE[1] + 22)
	$tempBalance = $tempBalance + $tempCalc

	Return $tempBalance
EndFunc   ;==>CheckBalance



Func SendKeyNumber($pay)

	If $pay == 1 Then
		Send("1")
	ElseIf $pay == 2 Then
		Send("2")
	ElseIf $pay == 3 Then
		Send("3")
	ElseIf $pay == 4 Then
		Send("4")
	ElseIf $pay == 5 Then
		Send("5")
	ElseIf $pay == 6 Then
		Send("6")
	ElseIf $pay == 7 Then
		Send("7")
	ElseIf $pay == 8 Then
		Send("8")
	ElseIf $pay == 9 Then
		Send("9")
	ElseIf $pay == 10 Then
		Send("10")
	ElseIf $pay == 11 Then
		Send("11")
	ElseIf $pay == 12 Then
		Send("12")
	ElseIf $pay == 13 Then
		Send("13")
	ElseIf $pay == 14 Then
		Send("14")
	ElseIf $pay == 15 Then
		Send("15")
	ElseIf $pay == 16 Then
		Send("16")
	ElseIf $pay == 17 Then
		Send("17")
	ElseIf $pay == 18 Then
		Send("18")
	ElseIf $pay == 19 Then
		Send("19")
	ElseIf $pay == 20 Then
		Send("20")
	ElseIf $pay == 21 Then
		Send("21")
	ElseIf $pay == 22 Then
		Send("22")
	ElseIf $pay == 23 Then
		Send("23")
	ElseIf $pay == 24 Then
		Send("24")
	ElseIf $pay == 25 Then
		Send("25")
	ElseIf $pay == 26 Then
		Send("26")
	ElseIf $pay == 27 Then
		Send("27")
	ElseIf $pay == 28 Then
		Send("28")
	ElseIf $pay == 29 Then
		Send("29")
	ElseIf $pay == 30 Then
		Send("30")
	ElseIf $pay == 31 Then
		Send("31")
	ElseIf $pay == 32 Then
		Send("32")
	ElseIf $pay == 33 Then
		Send("33")
	ElseIf $pay == 34 Then
		Send("34")
	ElseIf $pay == 35 Then
		Send("35")
	ElseIf $pay == 36 Then
		Send("36")
	ElseIf $pay == 37 Then
		Send("37")
	ElseIf $pay == 38 Then
		Send("38")
	ElseIf $pay == 39 Then
		Send("39")
	ElseIf $pay == 40 Then
		Send("40")
	ElseIf $pay == 41 Then
		Send("41")
	ElseIf $pay == 42 Then
		Send("42")
	ElseIf $pay == 43 Then
		Send("43")
	ElseIf $pay == 44 Then
		Send("44")
	ElseIf $pay == 45 Then
		Send("45")
	ElseIf $pay == 46 Then
		Send("46")
	ElseIf $pay == 47 Then
		Send("47")
	ElseIf $pay == 48 Then
		Send("48")
	ElseIf $pay == 49 Then
		Send("49")
	ElseIf $pay == 50 Then
		Send("50")
	ElseIf $pay == 51 Then
		Send("51")
	ElseIf $pay == 52 Then
		Send("52")
	ElseIf $pay == 53 Then
		Send("53")
	ElseIf $pay == 54 Then
		Send("54")
	ElseIf $pay == 55 Then
		Send("55")
	ElseIf $pay == 56 Then
		Send("56")
	ElseIf $pay == 57 Then
		Send("57")
	ElseIf $pay == 58 Then
		Send("58")
	ElseIf $pay == 59 Then
		Send("59")
	ElseIf $pay == 60 Then
		Send("60")
	ElseIf $pay == 61 Then
		Send("61")
	ElseIf $pay == 62 Then
		Send("62")
	ElseIf $pay == 63 Then
		Send("63")
	ElseIf $pay == 64 Then
		Send("64")
	ElseIf $pay == 65 Then
		Send("65")
	ElseIf $pay == 66 Then
		Send("66")
	ElseIf $pay == 67 Then
		Send("67")
	ElseIf $pay == 68 Then
		Send("68")
	ElseIf $pay == 69 Then
		Send("69")
	ElseIf $pay == 70 Then
		Send("70")
	ElseIf $pay == 71 Then
		Send("71")
	ElseIf $pay == 72 Then
		Send("72")
	ElseIf $pay == 73 Then
		Send("73")
	ElseIf $pay == 74 Then
		Send("74")
	ElseIf $pay == 75 Then
		Send("75")
	ElseIf $pay == 76 Then
		Send("76")
	ElseIf $pay == 77 Then
		Send("77")
	ElseIf $pay == 78 Then
		Send("78")
	ElseIf $pay == 79 Then
		Send("79")
	ElseIf $pay == 80 Then
		Send("80")
	ElseIf $pay == 81 Then
		Send("81")
	ElseIf $pay == 82 Then
		Send("82")
	ElseIf $pay == 83 Then
		Send("83")
	ElseIf $pay == 84 Then
		Send("84")
	ElseIf $pay == 85 Then
		Send("85")
	ElseIf $pay == 86 Then
		Send("86")
	ElseIf $pay == 87 Then
		Send("87")
	ElseIf $pay == 88 Then
		Send("88")
	ElseIf $pay == 89 Then
		Send("{8}{9}")
	ElseIf $pay == 90 Then
		Send("90")
	ElseIf $pay == 91 Then
		Send("91")
	ElseIf $pay == 92 Then
		Send("92")
	ElseIf $pay == 93 Then
		Send("93")
	ElseIf $pay == 94 Then
		Send("94")
	ElseIf $pay == 95 Then
		Send("95")
	ElseIf $pay == 96 Then
		Send("96")
	ElseIf $pay == 97 Then
		Send("97")
	ElseIf $pay == 98 Then
		Send("98")
	ElseIf $pay == 99 Then
		Send("99")
	ElseIf $pay == 100 Then
		Send("{1}{0}{0}")
	EndIf



EndFunc   ;==>SendKeyNumber


Func ResetPlayerData()

	$CURRENT_CHARACTER_NAME = "[No Player]"
	GUICtrlSetData($black_player, $CURRENT_CHARACTER_NAME)
	GUICtrlSetData($player_profit, 0 & " cc")

EndFunc   ;==>ResetPlayerData
Func SetPlayerData()

	Local $tempPlayerProfit
	Local $index

	For $index = 0 To UBound($PLAYER_LIST, 1) - 1
		If $PLAYER_LIST[$index][0] == $CURRENT_CHARACTER_NAME Then
			ExitLoop
		EndIf
	Next

	$tempPlayerProfit = $PLAYER_LIST[$index][1]
	GUICtrlSetData($player_profit, $tempPlayerProfit & " cc")

EndFunc   ;==>SetPlayerData
Func PlayerListAdd()


	Local $character_exist = False
	Local $last_index
	Local $index

	For $index = 0 To UBound($PLAYER_LIST, 1) - 1
		If $PLAYER_LIST[$index][0] == $CURRENT_CHARACTER_NAME Then
			$character_exist = True
			ExitLoop
		EndIf
	Next

	If Not $character_exist Then
		;====== $PLAYER_LIST #Array ===================================
		_ArrayAdd($PLAYER_LIST, $CURRENT_CHARACTER_NAME)
		$last_index = UBound($PLAYER_LIST, 1) - 1
		$PLAYER_LIST[$last_index][1] = 0
		$PLAYER_LIST[$last_index][2] = 0
		$PLAYER_LIST[$last_index][3] = 0
		$PLAYER_LIST[$last_index][4] = 0
		$PLAYER_LIST[$last_index][5] = 0

		;====== GameLog.ini ===========================================
		IniWrite($sFilePathGameLog, $CURRENT_CHARACTER_NAME, "Profit", 0)
		IniWrite($sFilePathGameLog, $CURRENT_CHARACTER_NAME, "Win", 0)
		IniWrite($sFilePathGameLog, $CURRENT_CHARACTER_NAME, "Lose", 0)
		IniWrite($sFilePathGameLog, $CURRENT_CHARACTER_NAME, "Visits", 0)
		IniWrite($sFilePathGameLog, $CURRENT_CHARACTER_NAME, "LastGame", 0)
	ElseIf $character_exist Then
		$PLAYER_LIST[$index][4] = $PLAYER_LIST[$index][4] + 1
	EndIf

EndFunc   ;==>PlayerListAdd
Func GameLogPlayerWin()

	Local $tempCurrentProfit
	Local $tempProfit
	Local $tempCurrentWin
	Local $tempWin
	Local $index

	For $index = 0 To UBound($PLAYER_LIST, 1) - 1
		If $PLAYER_LIST[$index][0] == $CURRENT_CHARACTER_NAME Then
			ExitLoop
		EndIf
	Next

	$tempCurrentProfit = $PLAYER_LIST[$index][1]
	$tempCurrentWin = $PLAYER_LIST[$index][2]
	$tempWin = $PLAYER_BET
	$tempProfit = $tempCurrentProfit + $tempWin

	;====== $PLAYER_LIST #Array ===================================
	$PLAYER_LIST[$index][1] = $tempProfit
	$PLAYER_LIST[$index][2] = $tempCurrentWin + $tempWin

	;====== GameLog.ini ===========================================
	IniWrite($sFilePathGameLog, $CURRENT_CHARACTER_NAME, "Profit", $PLAYER_LIST[$index][1])
	IniWrite($sFilePathGameLog, $CURRENT_CHARACTER_NAME, "Win", $PLAYER_LIST[$index][2])

EndFunc   ;==>GameLogPlayerWin
Func GameLogPlayerLost()

	Local $tempCurrentProfit
	Local $tempProfit
	Local $tempCurrentLose
	Local $tempLose
	Local $index

	For $index = 0 To UBound($PLAYER_LIST, 1) - 1
		If $PLAYER_LIST[$index][0] == $CURRENT_CHARACTER_NAME Then
			ExitLoop
		EndIf
	Next

	$tempCurrentProfit = $PLAYER_LIST[$index][1]
	$tempCurrentLose = $PLAYER_LIST[$index][3]
	$tempLose = $PLAYER_BET
	$tempProfit = $tempCurrentProfit - $tempLose

	;====== $PLAYER_LIST #Array ===================================
	$PLAYER_LIST[$index][1] = $tempProfit
	$PLAYER_LIST[$index][3] = $tempCurrentLose - $tempLose

	;====== GameLog.ini ===========================================
	IniWrite($sFilePathGameLog, $CURRENT_CHARACTER_NAME, "Profit", $PLAYER_LIST[$index][1])
	IniWrite($sFilePathGameLog, $CURRENT_CHARACTER_NAME, "Lose", $PLAYER_LIST[$index][3])

EndFunc   ;==>GameLogPlayerLost
Func GameLogChat($PlayerCall, $roll)

	Local $index

	For $index = 0 To UBound($PLAYER_LIST, 1) - 1
		If $PLAYER_LIST[$index][0] == $CURRENT_CHARACTER_NAME Then
			ExitLoop
		EndIf
	Next

	;====== $PLAYER_LIST #Array ===================================
	$PLAYER_LIST[$index][5] = _NowTime()

	;====== GameLog.txt #ChatLog ===========================================
	Local $hFileOpen = FileOpen($sFilePathGameLogChat, 1)
	FileWrite($hFileOpen, @CRLF & $PLAYER_LIST[$index][5] & " |" & $CURRENT_CHARACTER_NAME & ": " & $PlayerCall & " Casino: " & $roll)
	FileWrite($hFileOpen, @CRLF)
	FileClose($hFileOpen)

	;====== GameLog.ini ===========================================
	IniWrite($sFilePathGameLog, $CURRENT_CHARACTER_NAME, "Last Game", $PLAYER_LIST[$index][5])

EndFunc   ;==>GameLogChat
Func CheckIfScam($game)

	Local $scam

	Local $index

	For $index = 0 To UBound($PLAYER_LIST, 1) - 1
		If $PLAYER_LIST[$index][0] == $CURRENT_CHARACTER_NAME Then
			ExitLoop
		EndIf
	Next

	$CURRENT_BLACK_BALANCE = CheckBalance()

	If $game == "H/L" Then
		If $PLAYER_BET >= ($CURRENT_BLACK_BALANCE / 2) Then
			$scam = 1
		ElseIf $PLAYER_LIST[$index][1] >= 150 Then
			$scam = 1
		ElseIf $PLAYER_BET >= 110 Then
			$scam = 1
		Else
			$scam = Random(1, $SCAM_LEVEL, 1)
		EndIf
	ElseIf $game == "Numbers" Then
		If $PLAYER_BET >= ($CURRENT_BLACK_BALANCE / 5) Then
			$scam = 1
		ElseIf $PLAYER_LIST[$index][1] >= 150 Then
			$scam = 1
		ElseIf $PLAYER_BET >= 110 Then
			$scam = 1
		Else
			$scam = Random(1, $SCAM_LEVEL, 1)
		EndIf
	ElseIf $game == "BlackJack" Then
		If $PLAYER_BET >= ($CURRENT_BLACK_BALANCE / 2) Then
			$scam = 1
		ElseIf $PLAYER_BET >= 110 Then
			$scam = 1
		Else
			$scam = Random(1, $SCAM_LEVEL, 1)
		EndIf
	EndIf

	If $scam == 1 Then
		Return True
	Else
		Return False
	EndIf

EndFunc   ;==>CheckIfScam



Func GetCasinoProfit()

	Local $tempPlayerProfit
	Local $index
	Local $tempProfit = 0
	Local $casinoTempProfit = 0

	For $index = 0 To UBound($PLAYER_LIST, 1) - 1
		$tempProfit = $tempProfit + $PLAYER_LIST[$index][1]
	Next

	$casinoTempProfit = 0 - $tempProfit
	Return $casinoTempProfit

EndFunc   ;==>GetCasinoProfit
Func ProfitPerHour()

	$CASINO_PROFIT = GetCasinoProfit()

	Local $tempProfitHour = (3600 / (TimerDiff($BLACK_START_TIME) / 1000)) * $CASINO_PROFIT
	$tempProfitHour = StringFormat("%.2f", $tempProfitHour)
	Return $tempProfitHour
EndFunc   ;==>ProfitPerHour
Func UpdateProfitHourHUD()
	GUICtrlSetData($host_profit_per_hour, ProfitPerHour() & " cc")
EndFunc   ;==>UpdateProfitHourHUD


Func SelfSay($string)

	Sleep($MESSAGE_DELAY)
	Send($string, 1)
	Sleep(100)
	Send("{ENTER}")


EndFunc   ;==>SelfSay
Func PlayLoseAnimation()
;~ 	MouseClick($MOUSE_CLICK_RIGHT, $LOSE_ANIMATION_POSITION[0], $LOSE_ANIMATION_POSITION[1], 1, 10)
EndFunc   ;==>PlayLoseAnimation
Func PlayWinAnimation()
	MouseClick($MOUSE_CLICK_RIGHT, $WIN_ANIMATION_POSITION[0], $WIN_ANIMATION_POSITION[1], 1, 10)
EndFunc   ;==>PlayWinAnimation


Func CheckIfPlayerMessage()
	ConsoleWrite(@CRLF & "Start CheckIfPlayerMessage()")
	Local $topleftx = $CHAT_WINDOW_RECTANGLE[0]
	Local $toplefty = $CHAT_WINDOW_RECTANGLE[3] - 10
	Local $botrightx = $CHAT_WINDOW_RECTANGLE[0] + 10
	Local $botrighty = $CHAT_WINDOW_RECTANGLE[3]

	Local $aCoord = PixelSearch($topleftx, $toplefty, $botrightx, $botrighty, 0x60f8f8)
	If $aCoord <> 0 Then
		ConsoleWrite(@CRLF & "PLAYER MESSAGE DETECTED")
		Return True
	Else
		Return False
	EndIf

EndFunc   ;==>CheckIfPlayerMessage
Func CheckMessageToString($startX, $startY, $hDll, $vDC)
	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 8, $hDll) == "000000" Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckMessageToString
Func CheckCapitalLetters($startX, $startY, $hDll, $vDC)

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 8, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "A"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 9
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 7, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "B"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 8
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 7, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "C"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 8
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 6, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "D"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 9
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 8, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "E"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 8
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 4, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "F"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 8
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 8, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "G"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 9
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 8, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "H"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 9
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "I"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 6
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "J"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 6
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 8, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "K"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 8
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 9, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "L"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 7
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 8, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "M"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 10
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 8, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "N"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 9
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 7, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "O"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 9
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 4, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "P"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 8
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 9, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "R"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 8
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 7, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "S"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 8
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 2, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "T"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 8
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 7, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "U"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 9
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 3, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "V"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 8
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 10, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 10, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 10, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 10, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 10, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 10, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 10, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 11, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 11, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 11, $startY + 3, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "W"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 12
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 7, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "X"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 8
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 2, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "Y"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 8
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 8, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "Z"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 8
		Return True
	EndIf


	Return False
EndFunc   ;==>CheckCapitalLetters
Func CheckSmallLetters($startX, $startY, $hDll, $vDC)

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 8, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "a"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 8
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 7, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "b"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 8
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "c"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 7
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 8, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "d"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 8
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 7, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "e"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 8
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "f"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 5
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 9, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 9, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 9, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "g"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 8
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 8, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "h"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 8
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "i"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 4
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 9, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 9, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "j"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 5
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 8, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "k"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 8
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "l"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 4
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 10, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 10, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 10, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 10, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 10, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 10, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 11, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 11, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 11, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 11, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 11, $startY + 8, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "m"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 12
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 8, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "n"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 8
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 7, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "o"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 8
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 9, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 7, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "p"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 8
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "r"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 6
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "s"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 7
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "t"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 5
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 8, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "u"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 8
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 5, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "v"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 8
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 6, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "w"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 10
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 8, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "x"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 8
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 9, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 4, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "y"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 8
		Return True
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == "F7F7F7" And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 9, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) == "000000" Then
		$CURRENT_CHARACTER_NAME = $CURRENT_CHARACTER_NAME & "z"
		$STRING_BLOCK_SCAN_X = $STRING_BLOCK_SCAN_X + 7
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckSmallLetters
Func CheckCurrentMessageLetter($startX, $startY, $hDll, $vDC, $color)
	ConsoleWrite(@CRLF & "CHECK CHAT LETTER")
	Local $letter = ""

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == $color Then
		$letter = "a"
		$CURRENT_PLAYER_MSG = $CURRENT_PLAYER_MSG & "a"
		$PLAYER_MSG_STRING_BLOCK_SCAN_X = $PLAYER_MSG_STRING_BLOCK_SCAN_X + 8
		Return $letter
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == $color Then
		$letter = "b"
		$CURRENT_PLAYER_MSG = $CURRENT_PLAYER_MSG & "b"
		$PLAYER_MSG_STRING_BLOCK_SCAN_X = $PLAYER_MSG_STRING_BLOCK_SCAN_X + 8
		Return $letter
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == $color Then
		$letter = "c"
		$CURRENT_PLAYER_MSG = $CURRENT_PLAYER_MSG & "c"
		$PLAYER_MSG_STRING_BLOCK_SCAN_X = $PLAYER_MSG_STRING_BLOCK_SCAN_X + 7
		Return $letter
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == $color Then
		$letter = "d"
		$CURRENT_PLAYER_MSG = $CURRENT_PLAYER_MSG & "d"
		$PLAYER_MSG_STRING_BLOCK_SCAN_X = $PLAYER_MSG_STRING_BLOCK_SCAN_X + 8
		Return $letter
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == $color Then
		$letter = "e"
		$CURRENT_PLAYER_MSG = $CURRENT_PLAYER_MSG & "e"
		$PLAYER_MSG_STRING_BLOCK_SCAN_X = $PLAYER_MSG_STRING_BLOCK_SCAN_X + 8
		Return $letter
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) <> $color Then
		$letter = "f"
		$CURRENT_PLAYER_MSG = $CURRENT_PLAYER_MSG & "f"
		$PLAYER_MSG_STRING_BLOCK_SCAN_X = $PLAYER_MSG_STRING_BLOCK_SCAN_X + 5
		Return $letter
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 9, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 10, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 10, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 9, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 10, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 9, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 10, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 9, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 10, $hDll) <> $color Then
		$letter = "g"
		$CURRENT_PLAYER_MSG = $CURRENT_PLAYER_MSG & "g"
		$PLAYER_MSG_STRING_BLOCK_SCAN_X = $PLAYER_MSG_STRING_BLOCK_SCAN_X + 8
		Return $letter
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == $color Then
		$letter = "h"
		$CURRENT_PLAYER_MSG = $CURRENT_PLAYER_MSG & "h"
		$PLAYER_MSG_STRING_BLOCK_SCAN_X = $PLAYER_MSG_STRING_BLOCK_SCAN_X + 8
		Return $letter
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == $color Then
		$letter = "i"
		$CURRENT_PLAYER_MSG = $CURRENT_PLAYER_MSG & "i"
		$PLAYER_MSG_STRING_BLOCK_SCAN_X = $PLAYER_MSG_STRING_BLOCK_SCAN_X + 4
		Return $letter
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 9, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 10, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 9, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 10, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 10, $hDll) <> $color Then
		$letter = "j"
		$CURRENT_PLAYER_MSG = $CURRENT_PLAYER_MSG & "j"
		$PLAYER_MSG_STRING_BLOCK_SCAN_X = $PLAYER_MSG_STRING_BLOCK_SCAN_X + 5
		Return $letter
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == $color Then
		$letter = "k"
		$CURRENT_PLAYER_MSG = $CURRENT_PLAYER_MSG & "k"
		$PLAYER_MSG_STRING_BLOCK_SCAN_X = $PLAYER_MSG_STRING_BLOCK_SCAN_X + 8
		Return $letter
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) <> $color Then
		$letter = "l"
		$CURRENT_PLAYER_MSG = $CURRENT_PLAYER_MSG & "l"
		$PLAYER_MSG_STRING_BLOCK_SCAN_X = $PLAYER_MSG_STRING_BLOCK_SCAN_X + 4
		Return $letter
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 9, $startY + 8, $hDll) == $color Then
		$letter = "m"
		$CURRENT_PLAYER_MSG = $CURRENT_PLAYER_MSG & "m"
		$PLAYER_MSG_STRING_BLOCK_SCAN_X = $PLAYER_MSG_STRING_BLOCK_SCAN_X + 12
		Return $letter
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) <> $color Then
		$letter = "n"
		$CURRENT_PLAYER_MSG = $CURRENT_PLAYER_MSG & "n"
		$PLAYER_MSG_STRING_BLOCK_SCAN_X = $PLAYER_MSG_STRING_BLOCK_SCAN_X + 8
		Return $letter
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) <> $color Then
		$letter = "o"
		$CURRENT_PLAYER_MSG = $CURRENT_PLAYER_MSG & "o"
		$PLAYER_MSG_STRING_BLOCK_SCAN_X = $PLAYER_MSG_STRING_BLOCK_SCAN_X + 8
		Return $letter
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 9, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 10, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 9, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 10, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) <> $color Then
		$letter = "p"
		$CURRENT_PLAYER_MSG = $CURRENT_PLAYER_MSG & "p"
		$PLAYER_MSG_STRING_BLOCK_SCAN_X = $PLAYER_MSG_STRING_BLOCK_SCAN_X + 8
		Return $letter
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 9, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 10, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 9, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 10, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) <> $color Then
		$letter = "q"
		$CURRENT_PLAYER_MSG = $CURRENT_PLAYER_MSG & "q"
		$PLAYER_MSG_STRING_BLOCK_SCAN_X = $PLAYER_MSG_STRING_BLOCK_SCAN_X + 8
		Return $letter
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) <> $color Then
		$letter = "r"
		$CURRENT_PLAYER_MSG = $CURRENT_PLAYER_MSG & "r"
		$PLAYER_MSG_STRING_BLOCK_SCAN_X = $PLAYER_MSG_STRING_BLOCK_SCAN_X + 6
		Return $letter
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) <> $color Then
		$letter = "s"
		$CURRENT_PLAYER_MSG = $CURRENT_PLAYER_MSG & "s"
		$PLAYER_MSG_STRING_BLOCK_SCAN_X = $PLAYER_MSG_STRING_BLOCK_SCAN_X + 7
		Return $letter
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) <> $color Then
		$letter = "t"
		$CURRENT_PLAYER_MSG = $CURRENT_PLAYER_MSG & "t"
		$PLAYER_MSG_STRING_BLOCK_SCAN_X = $PLAYER_MSG_STRING_BLOCK_SCAN_X + 5
		Return $letter
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) <> $color Then
		$letter = "u"
		$CURRENT_PLAYER_MSG = $CURRENT_PLAYER_MSG & "u"
		$PLAYER_MSG_STRING_BLOCK_SCAN_X = $PLAYER_MSG_STRING_BLOCK_SCAN_X + 8
		Return $letter
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) <> $color Then
		$letter = "v"
		$CURRENT_PLAYER_MSG = $CURRENT_PLAYER_MSG & "v"
		$PLAYER_MSG_STRING_BLOCK_SCAN_X = $PLAYER_MSG_STRING_BLOCK_SCAN_X + 8
		Return $letter
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 7, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 8, $startY + 8, $hDll) <> $color Then
		$letter = "w"
		$CURRENT_PLAYER_MSG = $CURRENT_PLAYER_MSG & "w"
		$PLAYER_MSG_STRING_BLOCK_SCAN_X = $PLAYER_MSG_STRING_BLOCK_SCAN_X + 10
		Return $letter
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) <> $color Then
		$letter = "x"
		$CURRENT_PLAYER_MSG = $CURRENT_PLAYER_MSG & "x"
		$PLAYER_MSG_STRING_BLOCK_SCAN_X = $PLAYER_MSG_STRING_BLOCK_SCAN_X + 8
		Return $letter
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 9, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 10, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 10, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 6, $startY + 8, $hDll) <> $color Then
		$letter = "y"
		$CURRENT_PLAYER_MSG = $CURRENT_PLAYER_MSG & "y"
		$PLAYER_MSG_STRING_BLOCK_SCAN_X = $PLAYER_MSG_STRING_BLOCK_SCAN_X + 8
		Return $letter
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 9, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 10, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 9, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 10, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 9, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 10, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) <> $color Then
		$letter = "/"
		$CURRENT_PLAYER_MSG = $CURRENT_PLAYER_MSG & "/"
		$PLAYER_MSG_STRING_BLOCK_SCAN_X = $PLAYER_MSG_STRING_BLOCK_SCAN_X + 8
		Return $letter
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == $color Then
		$letter = "1"
		$CURRENT_PLAYER_MSG = $CURRENT_PLAYER_MSG & "1"
		$PLAYER_MSG_STRING_BLOCK_SCAN_X = $PLAYER_MSG_STRING_BLOCK_SCAN_X + 8
		Return $letter
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) == $color Then
		$letter = "2"
		$CURRENT_PLAYER_MSG = $CURRENT_PLAYER_MSG & "2"
		$PLAYER_MSG_STRING_BLOCK_SCAN_X = $PLAYER_MSG_STRING_BLOCK_SCAN_X + 8
		Return $letter
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) <> $color Then
		$letter = "3"
		$CURRENT_PLAYER_MSG = $CURRENT_PLAYER_MSG & "3"
		$PLAYER_MSG_STRING_BLOCK_SCAN_X = $PLAYER_MSG_STRING_BLOCK_SCAN_X + 8
		Return $letter
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) <> $color Then
		$letter = "4"
		$CURRENT_PLAYER_MSG = $CURRENT_PLAYER_MSG & "4"
		$PLAYER_MSG_STRING_BLOCK_SCAN_X = $PLAYER_MSG_STRING_BLOCK_SCAN_X + 8
		Return $letter
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) <> $color Then
		$letter = "5"
		$CURRENT_PLAYER_MSG = $CURRENT_PLAYER_MSG & "5"
		$PLAYER_MSG_STRING_BLOCK_SCAN_X = $PLAYER_MSG_STRING_BLOCK_SCAN_X + 8
		Return $letter
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) <> $color Then
		$letter = "6"
		$CURRENT_PLAYER_MSG = $CURRENT_PLAYER_MSG & "6"
		$PLAYER_MSG_STRING_BLOCK_SCAN_X = $PLAYER_MSG_STRING_BLOCK_SCAN_X + 8
		Return $letter
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) <> $color Then
		$letter = "7"
		$CURRENT_PLAYER_MSG = $CURRENT_PLAYER_MSG & "7"
		$PLAYER_MSG_STRING_BLOCK_SCAN_X = $PLAYER_MSG_STRING_BLOCK_SCAN_X + 8
		Return $letter
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) <> $color Then
		$letter = "8"
		$CURRENT_PLAYER_MSG = $CURRENT_PLAYER_MSG & "8"
		$PLAYER_MSG_STRING_BLOCK_SCAN_X = $PLAYER_MSG_STRING_BLOCK_SCAN_X + 8
		Return $letter
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) <> $color Then
		$letter = "9"
		$CURRENT_PLAYER_MSG = $CURRENT_PLAYER_MSG & "9"
		$PLAYER_MSG_STRING_BLOCK_SCAN_X = $PLAYER_MSG_STRING_BLOCK_SCAN_X + 8
		Return $letter
	EndIf

	If _PixelGetColor_GetPixel($vDC, $startX, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX, $startY + 8, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 1, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 2, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 2, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 3, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 4, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 5, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 6, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 7, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 3, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 1, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 4, $startY + 8, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 1, $hDll) <> $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 2, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 3, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 4, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 5, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 6, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 7, $hDll) == $color And _
			_PixelGetColor_GetPixel($vDC, $startX + 5, $startY + 8, $hDll) <> $color Then
		$letter = "0"
		$CURRENT_PLAYER_MSG = $CURRENT_PLAYER_MSG & "0"
		$PLAYER_MSG_STRING_BLOCK_SCAN_X = $PLAYER_MSG_STRING_BLOCK_SCAN_X + 8
		Return $letter
	EndIf

	Return False
EndFunc   ;==>CheckCurrentMessageLetter

Func CheckIfEmptySlot($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "262627" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "2A2A2B" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "2B2C2B" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "272828" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "272728" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "2C2C2C" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfEmptySlot
;====== BET ITEMS =======
Func CheckIfCrystalCoin($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_cc = 0

	;===== 1 cc =====
	If _PixelGetColor_GetPixel($vDC, $x + 13, $y + 10, $hDll) == "03177E" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 10, $hDll) == "0874DA" Then
		$found_cc = 1
		;===== 2 cc =====
	ElseIf _PixelGetColor_GetPixel($vDC, $x + 15, $y + 9, $hDll) == "03177F" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 9, $hDll) == "0874DA" Then
		$found_cc = 1
		;===== 3 cc =====
	ElseIf _PixelGetColor_GetPixel($vDC, $x + 6, $y + 10, $hDll) == "03177E" And _
			_PixelGetColor_GetPixel($vDC, $x + 7, $y + 10, $hDll) == "0874D9" Then
		$found_cc = 1
		;===== 4 cc =====
	ElseIf _PixelGetColor_GetPixel($vDC, $x + 7, $y + 6, $hDll) == "03177F" And _
			_PixelGetColor_GetPixel($vDC, $x + 8, $y + 6, $hDll) == "0875DB" Then
		$found_cc = 1
		;===== 5+ cc =====
	ElseIf _PixelGetColor_GetPixel($vDC, $x + 3, $y + 17, $hDll) == "03177F" And _
			_PixelGetColor_GetPixel($vDC, $x + 4, $y + 17, $hDll) == "0874D9" Then
		$found_cc = 1
		;===== 10+ cc =====
	ElseIf _PixelGetColor_GetPixel($vDC, $x + 12, $y + 15, $hDll) == "0427A3" And _
			_PixelGetColor_GetPixel($vDC, $x + 13, $y + 15, $hDll) == "0764D0" Then
		$found_cc = 1
		;===== 25+ cc =====
	ElseIf _PixelGetColor_GetPixel($vDC, $x + 12, $y + 15, $hDll) == "0C9DE8" And _
			_PixelGetColor_GetPixel($vDC, $x + 13, $y + 15, $hDll) == "04269D" Then
		$found_cc = 1
		;===== 50+ cc =====
	ElseIf _PixelGetColor_GetPixel($vDC, $x + 12, $y + 15, $hDll) == "52EEFB" And _
			_PixelGetColor_GetPixel($vDC, $x + 13, $y + 15, $hDll) == "96FCFC" Then
		$found_cc = 1
	EndIf


	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_cc == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfCrystalCoin
Func CheckIf100PlatCoins($x, $y)

	Local $numb
	Local $found_100pc = 0

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	If _PixelGetColor_GetPixel($vDC, $x + 12, $y + 15, $hDll) == "AEB7D3" And _
			_PixelGetColor_GetPixel($vDC, $x + 13, $y + 15, $hDll) == "D1D8EC" Then
		$found_100pc = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	$numb = CountPlatinCoins($x + 9, $y + 22)

	If $found_100pc == 1 And $numb = 100 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIf100PlatCoins
Func CheckIfBootsOfHaste($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 10, $y + 9, $hDll) == "A7604A" And _
			_PixelGetColor_GetPixel($vDC, $x + 11, $y + 9, $hDll) == "A7604A" And _
			_PixelGetColor_GetPixel($vDC, $x + 12, $y + 9, $hDll) == "9A5A3B" And _
			_PixelGetColor_GetPixel($vDC, $x + 10, $y + 10, $hDll) == "85351D" And _
			_PixelGetColor_GetPixel($vDC, $x + 11, $y + 10, $hDll) == "601C0A" And _
			_PixelGetColor_GetPixel($vDC, $x + 12, $y + 10, $hDll) == "85351D" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfBootsOfHaste
Func CheckIfDemonArmor($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 10, $y + 10, $hDll) == "A60503" And _
			_PixelGetColor_GetPixel($vDC, $x + 11, $y + 10, $hDll) == "4B0202" And _
			_PixelGetColor_GetPixel($vDC, $x + 12, $y + 10, $hDll) == "7D0303" And _
			_PixelGetColor_GetPixel($vDC, $x + 10, $y + 11, $hDll) == "420000" And _
			_PixelGetColor_GetPixel($vDC, $x + 11, $y + 11, $hDll) == "CD0200" And _
			_PixelGetColor_GetPixel($vDC, $x + 12, $y + 11, $hDll) == "E20300" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfDemonArmor
Func CheckIfDemonHelmet($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 10, $y + 10, $hDll) == "EB7B7B" And _
			_PixelGetColor_GetPixel($vDC, $x + 11, $y + 10, $hDll) == "ED918E" And _
			_PixelGetColor_GetPixel($vDC, $x + 12, $y + 10, $hDll) == "EC9996" And _
			_PixelGetColor_GetPixel($vDC, $x + 10, $y + 11, $hDll) == "EB7072" And _
			_PixelGetColor_GetPixel($vDC, $x + 11, $y + 11, $hDll) == "EC7A7A" And _
			_PixelGetColor_GetPixel($vDC, $x + 12, $y + 11, $hDll) == "EA7E7F" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfDemonHelmet
Func CheckIfBlueRobe($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 10, $y + 10, $hDll) == "061E49" And _
			_PixelGetColor_GetPixel($vDC, $x + 11, $y + 10, $hDll) == "04122B" And _
			_PixelGetColor_GetPixel($vDC, $x + 12, $y + 10, $hDll) == "05142B" And _
			_PixelGetColor_GetPixel($vDC, $x + 10, $y + 11, $hDll) == "002050" And _
			_PixelGetColor_GetPixel($vDC, $x + 11, $y + 11, $hDll) == "010B21" And _
			_PixelGetColor_GetPixel($vDC, $x + 12, $y + 11, $hDll) == "061633" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfBlueRobe
Func CheckIfMasterMindShield($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 10, $y + 10, $hDll) == "676E55" And _
			_PixelGetColor_GetPixel($vDC, $x + 11, $y + 10, $hDll) == "A6A584" And _
			_PixelGetColor_GetPixel($vDC, $x + 12, $y + 10, $hDll) == "212C22" And _
			_PixelGetColor_GetPixel($vDC, $x + 10, $y + 11, $hDll) == "9B9B7D" And _
			_PixelGetColor_GetPixel($vDC, $x + 11, $y + 11, $hDll) == "B1A28D" And _
			_PixelGetColor_GetPixel($vDC, $x + 12, $y + 11, $hDll) == "2E3D36" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfMasterMindShield
Func CheckIfGoldenLegs($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 10, $y + 10, $hDll) == "785218" And _
			_PixelGetColor_GetPixel($vDC, $x + 11, $y + 10, $hDll) == "432909" And _
			_PixelGetColor_GetPixel($vDC, $x + 12, $y + 10, $hDll) == "4D330C" And _
			_PixelGetColor_GetPixel($vDC, $x + 10, $y + 11, $hDll) == "7C4B18" And _
			_PixelGetColor_GetPixel($vDC, $x + 11, $y + 11, $hDll) == "371F07" And _
			_PixelGetColor_GetPixel($vDC, $x + 12, $y + 11, $hDll) == "3C2609" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfGoldenLegs
Func CheckIfGoldenArmor($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 10, $y + 10, $hDll) == "F6B93A" And _
			_PixelGetColor_GetPixel($vDC, $x + 11, $y + 10, $hDll) == "FDD84D" And _
			_PixelGetColor_GetPixel($vDC, $x + 12, $y + 10, $hDll) == "FCF870" And _
			_PixelGetColor_GetPixel($vDC, $x + 10, $y + 11, $hDll) == "FDED68" And _
			_PixelGetColor_GetPixel($vDC, $x + 11, $y + 11, $hDll) == "EAA035" And _
			_PixelGetColor_GetPixel($vDC, $x + 12, $y + 11, $hDll) == "EAA32C" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfGoldenArmor
Func CheckIfDemonShield($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 10, $y + 10, $hDll) == "D70000" And _
			_PixelGetColor_GetPixel($vDC, $x + 11, $y + 10, $hDll) == "980000" And _
			_PixelGetColor_GetPixel($vDC, $x + 12, $y + 10, $hDll) == "B70000" And _
			_PixelGetColor_GetPixel($vDC, $x + 10, $y + 11, $hDll) == "A50000" And _
			_PixelGetColor_GetPixel($vDC, $x + 11, $y + 11, $hDll) == "B30007" And _
			_PixelGetColor_GetPixel($vDC, $x + 12, $y + 11, $hDll) == "C50000" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfDemonShield
Func CheckIfAmuletOfLoss($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 15, $y + 22, $hDll) == "5F5072" And _
			_PixelGetColor_GetPixel($vDC, $x + 11, $y + 10, $hDll) == "E97FDB" And _
			_PixelGetColor_GetPixel($vDC, $x + 12, $y + 10, $hDll) == "EF89DF" And _
			_PixelGetColor_GetPixel($vDC, $x + 10, $y + 11, $hDll) == "C353B5" And _
			_PixelGetColor_GetPixel($vDC, $x + 11, $y + 11, $hDll) == "E670D7" And _
			_PixelGetColor_GetPixel($vDC, $x + 12, $y + 11, $hDll) == "EC78DD" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfAmuletOfLoss
Func CheckIfMagicPlateArmor($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 10, $y + 10, $hDll) == "2B241D" And _
			_PixelGetColor_GetPixel($vDC, $x + 11, $y + 10, $hDll) == "93AAA4" And _
			_PixelGetColor_GetPixel($vDC, $x + 12, $y + 10, $hDll) == "FFFFFF" And _
			_PixelGetColor_GetPixel($vDC, $x + 10, $y + 11, $hDll) == "48463C" And _
			_PixelGetColor_GetPixel($vDC, $x + 11, $y + 11, $hDll) == "4C565D" And _
			_PixelGetColor_GetPixel($vDC, $x + 12, $y + 11, $hDll) == "7694A7" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfMagicPlateArmor
Func CheckIfRoyalHelmet($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 10, $y + 10, $hDll) == "98260E" And _
			_PixelGetColor_GetPixel($vDC, $x + 11, $y + 10, $hDll) == "270100" And _
			_PixelGetColor_GetPixel($vDC, $x + 12, $y + 10, $hDll) == "58312C" And _
			_PixelGetColor_GetPixel($vDC, $x + 10, $y + 11, $hDll) == "25080C" And _
			_PixelGetColor_GetPixel($vDC, $x + 11, $y + 11, $hDll) == "776966" And _
			_PixelGetColor_GetPixel($vDC, $x + 12, $y + 11, $hDll) == "3B2E26" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfRoyalHelmet
Func CheckIfSteelBoots($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 10, $y + 10, $hDll) == "C0C4BE" And _
			_PixelGetColor_GetPixel($vDC, $x + 11, $y + 10, $hDll) == "6C7B68" And _
			_PixelGetColor_GetPixel($vDC, $x + 12, $y + 10, $hDll) == "909F8A" And _
			_PixelGetColor_GetPixel($vDC, $x + 10, $y + 11, $hDll) == "BEC2BC" And _
			_PixelGetColor_GetPixel($vDC, $x + 11, $y + 11, $hDll) == "9AA896" And _
			_PixelGetColor_GetPixel($vDC, $x + 12, $y + 11, $hDll) == "909F8A" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfSteelBoots
Func CheckIfSoftBoots($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 12, $y + 12, $hDll) == "775CB0" And _
			_PixelGetColor_GetPixel($vDC, $x + 13, $y + 12, $hDll) == "7358AC" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 12, $hDll) == "4F3488" And _
			_PixelGetColor_GetPixel($vDC, $x + 12, $y + 13, $hDll) == "5C4195" And _
			_PixelGetColor_GetPixel($vDC, $x + 13, $y + 13, $hDll) == "4F3488" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 13, $hDll) == "2F1468" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfSoftBoots
Func CheckIfEliteDrakenHelmet($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "925200" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "DEAE1C" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "CC9300" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "563102" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "CC9300" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "CC9300" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfEliteDrakenHelmet
Func CheckIfHatOfTheMad($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "7E3D60" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "703555" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "733757" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "65304C" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "65304C" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "65304C" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfHatOfTheMad
Func CheckIfZaoanHelmet($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "A46D13" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "0E0803" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "734005" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "CD9B2F" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "0E0803" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "CD9B2F" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfZaoanHelmet
Func CheckIfYalahariMask($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "B96C06" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "CE0000" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "B06707" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "FBFBC7" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "BC7F26" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "FDFDE4" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfYalahariMask
Func CheckIfMasterArchersArmor($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "71231A" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "491814" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "390F0C" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "371916" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "2E1412" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "2F110E" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfMasterArchersArmor
Func CheckIfPaladinArmor($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "595959" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "202020" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "595959" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "343434" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "1A1A1A" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "343434" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfPaladinArmor
Func CheckIfRoyalScaleRobe($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "3E2402" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "3E2402" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "563102" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "744101" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "3E2402" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "925200" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfRoyalScaleRobe
Func CheckIfRoyalDrakenMail($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "744101" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "925200" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "744101" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "CC9300" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "925200" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "CC9300" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfRoyalDrakenMail
Func CheckIfEliteDrakenMail($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "985298" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "985298" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "5c5c5c" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "000000" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfEliteDrakenMail
Func CheckIfBastSkirt($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "0F5D3E" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "816538" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "84683A" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "024B2D" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "17945E" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "024726" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfBastSkirt
Func CheckIfBlueLegs($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "203D9C" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "0D1C4A" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "84683A" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "020716" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "040E2C" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "061442" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfBlueLegs
Func CheckIfYalahariLegPiece($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "E9C61A" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "C7672F" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "E5C119" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "E9C61A" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "CB6A31" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "E9C61A" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfYalahariLegPiece
Func CheckIfAbyssHammer($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "9A8696" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "4D434B" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "6F647A" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "131113" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfAbyssHammer
Func CheckIfArbalest($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "A97f6D" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "B07A63" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "7B4930" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "432A1B" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "3B2217" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "5F3318" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfArbalest
Func CheckIfArcaneStaff($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "090000" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "5E4322" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "3E290C" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfArcaneStaff
Func CheckIfAssassinDagger($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "090000" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "5E4322" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "3E290C" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfAssassinDagger
Func CheckIfBananaStaff($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "6D2700" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "8D1C00" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "020100" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "DD3000" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "4B1900" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfBananaStaff
Func CheckIfBerserker($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "EEECEA" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "9E9186" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "2F2B2A" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "C5BEB7" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "EFEDEB" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "9E9186" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfBerserker
Func CheckIfBladeOfCorruption($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "C47DA3" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "423E43" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "423E43" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "C47DA3" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "6E6970" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "423E43" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfBladeOfCorruption
Func CheckIfBlessedSceptre($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "E1B039" And _
			_PixelGetColor_GetPixel($vDC, $x + 17, $y + 14, $hDll) == "E1B039" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "E1B039" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "E7BE54" And _
			_PixelGetColor_GetPixel($vDC, $x + 17, $y + 15, $hDll) == "ECC96A" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfBlessedSceptre
Func CheckIfBloodyEdge($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 17, $y + 14, $hDll) == "565D6E" And _
			_PixelGetColor_GetPixel($vDC, $x + 18, $y + 14, $hDll) == "4B5160" And _
			_PixelGetColor_GetPixel($vDC, $x + 19, $y + 14, $hDll) == "505666" And _
			_PixelGetColor_GetPixel($vDC, $x + 17, $y + 15, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $x + 18, $y + 15, $hDll) == "565D6E" And _
			_PixelGetColor_GetPixel($vDC, $x + 19, $y + 15, $hDll) == "848CA4" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfBloodyEdge
Func CheckIfCrystallineAxe($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "4A0101" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "F7B524" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "CC8fF0A" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "663200" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "FFE7B1" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "F7B524" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfCrystallineAxe
Func CheckIfDeeplingAxe($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "244445" And _
			_PixelGetColor_GetPixel($vDC, $x + 17, $y + 14, $hDll) == "132929" And _
			_PixelGetColor_GetPixel($vDC, $x + 18, $y + 14, $hDll) == "F1C44B" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $x + 17, $y + 15, $hDll) == "244445" And _
			_PixelGetColor_GetPixel($vDC, $x + 18, $y + 15, $hDll) == "132929" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfDeeplingAxe
Func CheckIfDemonbone($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "6A0E10" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "660B0D" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "460A0B" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "BA3538" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "AF4547" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "6B2224" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfDemonbone
Func CheckIfDjinnBlade($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "B6B6B1" And _
			_PixelGetColor_GetPixel($vDC, $x + 17, $y + 14, $hDll) == "7C7C77" And _
			_PixelGetColor_GetPixel($vDC, $x + 18, $y + 14, $hDll) == "AAAAAF" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $x + 17, $y + 15, $hDll) == "BFBFBA" And _
			_PixelGetColor_GetPixel($vDC, $x + 18, $y + 15, $hDll) == "70706D" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfDjinnBlade
Func CheckIfGlacialRod($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "88C6E6" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "306D8D" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "88C6E6" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "306D8D" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "114560" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "FFE7B1" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfGlacialRod
Func CheckIfGuardianHalberd($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "DFA76C" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "AF5E1C" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "AF5E1C" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "AD6628" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "DBD4CD" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "828282" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfGuardianHalberd
Func CheckIfHammerOfWrath($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "363636" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "643C02" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "603A02" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "633B03" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "5C3703" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfHammerOfWrath
Func CheckIfHeavyMace($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "07170F" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "718179" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "35413B" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "7C8C84" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "C2C6C4" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "1B231F" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfHeavyMace
Func CheckIfHellforgedAxe($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $x + 17, $y + 14, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $x + 17, $y + 15, $hDll) == "0C080B" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfHellforgedAxe
Func CheckIfHeroicAxe($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "2B2B2B" And _
			_PixelGetColor_GetPixel($vDC, $x + 17, $y + 14, $hDll) == "767676" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "767676" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "767676" And _
			_PixelGetColor_GetPixel($vDC, $x + 17, $y + 15, $hDll) == "D9D9D9" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfHeroicAxe
Func CheckIfImpaler($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "2B3937" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "3F5351" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "354645" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "000000" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfImpaler
Func CheckIfMagicSword($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "676767" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "7B7B7B" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "484848" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "717171" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "676767" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "7B7B7B" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfMagicSword
Func CheckIfMaimer($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "410A0B" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "3C2D2E" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "3C2D2E" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "6F7B81" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "5D5C54" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "384040" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfMaimer
Func CheckIfMysticBlade($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "7B8192" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "555A67" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "CED0D6" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "4D515D" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "CDCFD6" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "4E525E" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfMysticBlade
Func CheckIfNightmareBlade($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "BCB4AE" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "B3A8A0" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "958B85" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "6E7371" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "BCB4AE" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "C4BBB5" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfNightmareBlade
Func CheckIfNobleAxe($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "9C5D52" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "003C52" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "637D00" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "003C52" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "637D00" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "003C52" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfNobleAxe
Func CheckIfNorthernStar($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "537AAD" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "1A2B59" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "060A16" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "274185" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "638CAB" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "537AAD" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfNorthernStar
Func CheckIfOrnamentedAxe($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 12, $y + 14, $hDll) == "A9A49D" And _
			_PixelGetColor_GetPixel($vDC, $x + 13, $y + 14, $hDll) == "7C7770" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $x + 12, $y + 15, $hDll) == "38332C" And _
			_PixelGetColor_GetPixel($vDC, $x + 13, $y + 15, $hDll) == "3A352E" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "000000" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfOrnamentedAxe
Func CheckIfOrnateCrossbow($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "266051" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "2D705F" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "1C463B" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "205044" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "266051" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "706F53" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfOrnateCrossbow
Func CheckIfOrnateMace($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "F6FAFE" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "C3C8CC" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "55606B" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "9A9DA0" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "F6FAFE" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "55606B" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfOrnateMace
Func CheckIfQueensSceptre($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "806C10" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "282206" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "806C10" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "403608" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "DFDFDF" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "403608" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfQueensSceptre
Func CheckIfShadowSceptre($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $x + 17, $y + 14, $hDll) == "3A506B" And _
			_PixelGetColor_GetPixel($vDC, $x + 18, $y + 14, $hDll) == "53708F" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "8D98A6" And _
			_PixelGetColor_GetPixel($vDC, $x + 17, $y + 15, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $x + 18, $y + 15, $hDll) == "000000" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfShadowSceptre
Func CheckIfShinyBlade($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "0CDBE4" And _
			_PixelGetColor_GetPixel($vDC, $x + 17, $y + 14, $hDll) == "1B8CFF" And _
			_PixelGetColor_GetPixel($vDC, $x + 18, $y + 14, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "0CDBE4" And _
			_PixelGetColor_GetPixel($vDC, $x + 17, $y + 15, $hDll) == "0CDBE4" And _
			_PixelGetColor_GetPixel($vDC, $x + 18, $y + 15, $hDll) == "139FFE" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfShinyBlade







;======== ULTRA RARE ITEM LIST =============
Func CheckIfAlbinoPlate($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "635E58" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "4C0014" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "2C000C" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "4C0014" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "FFD0DC" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "BC0031" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfAlbinoPlate
Func CheckIfOrnateChestPlate($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "06276C" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "215ACD" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "1040A2" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "17252C" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "021131" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "021131" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfOrnateChestPlate
Func CheckIfEarthmindRaiment($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "917295" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "917295" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "7F535C" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "7F535C" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "7F535C" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "693136" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfEarthmindRaiment
Func CheckIfFirebornGiantArmor($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "AD3205" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "3E1502" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "CB4608" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "671301" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "341001" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "EB540A" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfFirebornGiantArmor
Func CheckIfFiremindRaiment($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 8, $y + 14, $hDll) == "7A1B1D" And _
			_PixelGetColor_GetPixel($vDC, $x + 9, $y + 14, $hDll) == "4B0A19" And _
			_PixelGetColor_GetPixel($vDC, $x + 10, $y + 14, $hDll) == "7A1B1F" And _
			_PixelGetColor_GetPixel($vDC, $x + 8, $y + 15, $hDll) == "7A1B1F" And _
			_PixelGetColor_GetPixel($vDC, $x + 9, $y + 15, $hDll) == "631317" And _
			_PixelGetColor_GetPixel($vDC, $x + 10, $y + 15, $hDll) == "AF242E" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfFiremindRaiment
Func CheckIfFrostmindRaiment($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 8, $y + 14, $hDll) == "00546A" And _
			_PixelGetColor_GetPixel($vDC, $x + 9, $y + 14, $hDll) == "003E40" And _
			_PixelGetColor_GetPixel($vDC, $x + 10, $y + 14, $hDll) == "00546A" And _
			_PixelGetColor_GetPixel($vDC, $x + 8, $y + 15, $hDll) == "00546A" And _
			_PixelGetColor_GetPixel($vDC, $x + 9, $y + 15, $hDll) == "004456" And _
			_PixelGetColor_GetPixel($vDC, $x + 10, $y + 15, $hDll) == "007D98" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfFrostmindRaiment
Func CheckIfMoltenPlate($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "A22707" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "FFE510" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "550C02" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "A02707" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "FFE835" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "580D02" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfMoltenPlate
Func CheckIfShieldOfCorruption($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "D39DBA" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "706B71" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "37333A" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "555058" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "706B71" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "37333A" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfShieldOfCorruption
Func CheckIfAmazonArmor($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "3D0F58" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "310D55" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "5C0A76" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "230C38" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "160822" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "43015B" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfAmazonArmor
Func CheckIfBallGown($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "774701" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "B9863D" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "613C04" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "38945F" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "4AA872" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "27854F" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfBallGown
Func CheckIfDarkLordsCape($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "555555" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "3F3F3F" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "3F3F3F" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "090909" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "0D0D0D" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "0D0D0D" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfDarkLordsCape
Func CheckIfDepthLorica($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "1C1503" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "1C1503" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "1C1503" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "FBE5AB" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "FBE5AB" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "F1C44B" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfDepthLorica
Func CheckIfDwarvenArmor($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "EDEDED" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "727272" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "979797" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "DADADA" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "6B6B6B" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "7F7F7F" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfDwarvenArmor
Func CheckIfEarthbornTitanArmor($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "5BA342" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "162C13" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "4C9141" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "426F2B" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "12240F" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "88D568" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfEarthbornTitanArmor
Func CheckIfFuriousFrock($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "39170F" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "39170F" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "39170F" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "39170F" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "39170F" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "602B14" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfFuriousFrock
Func CheckIfGooShell($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "DFF1E6" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "9BDEB8" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "4AAC6C" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "9BDEB8" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "4AAC6C" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "4AAC6C" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfGooShell
Func CheckIfOceanbornLeviathanArmor($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "009DD9" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "006E91" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "1B2323" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "0099D0" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "1A5B6A" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "1B2323" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfOceanbornLeviathanArmor
Func CheckIfRobeOfTheIceQueen($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "060D3D" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "160A2C" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "190B33" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "2156AB" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "3FD7EB" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "24A8DE" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfRobeOfTheIceQueen
Func CheckIfRobeOfTheUnderworld($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "F0F0EA" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "D7D7D0" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "C7C7C3" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "CACAC6" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "F4F4EF" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "A0A094" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfRobeOfTheUnderworld
Func CheckIfVelvetMantle($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "85429E" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "14051D" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "170522" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "1E082A" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "150520" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "150520" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfVelvetMantle
Func CheckIfShrunkenHeadNecklace($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 20, $hDll) == "475914" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 20, $hDll) == "181D03" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 20, $hDll) == "242C07" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 21, $hDll) == "181D03" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 21, $hDll) == "242C07" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 21, $hDll) == "3b4410" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfShrunkenHeadNecklace
Func CheckIfBunnyslippers($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "A2773B" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "A57A3B" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "000000" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfBunnyslippers
Func CheckIfCrystalBoots($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 10, $y + 14, $hDll) == "3E3EAA" And _
			_PixelGetColor_GetPixel($vDC, $x + 11, $y + 14, $hDll) == "7280C3" And _
			_PixelGetColor_GetPixel($vDC, $x + 12, $y + 14, $hDll) == "505B8D" And _
			_PixelGetColor_GetPixel($vDC, $x + 10, $y + 15, $hDll) == "565BBB" And _
			_PixelGetColor_GetPixel($vDC, $x + 11, $y + 15, $hDll) == "7684C3" And _
			_PixelGetColor_GetPixel($vDC, $x + 12, $y + 15, $hDll) == "525AA5" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfCrystalBoots
Func CheckIfDragonScaleBoots($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 10, $y + 14, $hDll) == "001B30" And _
			_PixelGetColor_GetPixel($vDC, $x + 11, $y + 14, $hDll) == "071504" And _
			_PixelGetColor_GetPixel($vDC, $x + 12, $y + 14, $hDll) == "12300D" And _
			_PixelGetColor_GetPixel($vDC, $x + 10, $y + 15, $hDll) == "183E12" And _
			_PixelGetColor_GetPixel($vDC, $x + 11, $y + 15, $hDll) == "204E17" And _
			_PixelGetColor_GetPixel($vDC, $x + 12, $y + 15, $hDll) == "14320E" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfDragonScaleBoots
Func CheckIfGoldenBoots($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 2, $y + 22, $hDll) == "FFF1B1" And _
			_PixelGetColor_GetPixel($vDC, $x + 3, $y + 22, $hDll) == "FFF7D0" And _
			_PixelGetColor_GetPixel($vDC, $x + 4, $y + 22, $hDll) == "FFEA6A" And _
			_PixelGetColor_GetPixel($vDC, $x + 2, $y + 23, $hDll) == "FFB616" And _
			_PixelGetColor_GetPixel($vDC, $x + 3, $y + 23, $hDll) == "FFC065" And _
			_PixelGetColor_GetPixel($vDC, $x + 4, $y + 23, $hDll) == "F48C00" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfGoldenBoots
Func CheckIfTreaderOfTorment($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "201B21" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "3D3C42" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "201B21" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "272429" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "3D3C42" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "201B21" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfTreaderOfTorment
Func CheckIfVampireSilkSlippers($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "3D3539" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "595155" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "17181D" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "595155" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "17181D" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "2F282D" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfVampireSilkSlippers
Func CheckIfWingedHelmet($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "FFFFC4" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "FFFFC4" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "FFED31" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "FFFFFF" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "F6F6FF" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "DFDFF6" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfWingedHelmet
Func CheckIfWerewolfHelmet($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "4A3F3C" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "35302B" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "6E5D54" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "2F2E2B" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "423B38" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "423B38" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfWerewolfHelmet
Func CheckIfVisageOfTheEndDays($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "1B0615" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "521208" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "B31004" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "A4B3BF" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "1B0615" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "1B0615" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfVisageOfTheEndDays
Func CheckIfGoldenHelmet($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "2A1B03" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "382B0A" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "362D0A" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "D3BE31" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "251B05" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "1E1503" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfGoldenHelmet
Func CheckIfDwarvenHelmet($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "726A5E" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "D7D3CD" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "766E62" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "CFCBC5" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "EBE7E1" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "D5D1CB" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfDwarvenHelmet
Func CheckIfDragonScaleHelmet($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "CA7433" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "CE9A67" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "461C0F" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "DC6226" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "FCBA4D" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "9E4A26" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfDragonScaleHelmet
Func CheckIfAmazonHelmet($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "D98AF7" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "933EA6" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "510047" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "B061CE" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "B563CF" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "5B0A66" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfAmazonHelmet
Func CheckIfAncientTiara($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "FFEB00" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "FFD000" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "F59000" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfAncientTiara
Func CheckIfDemonLegs($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 11, $y + 1, $hDll) == "130000" And _
			_PixelGetColor_GetPixel($vDC, $x + 12, $y + 1, $hDll) == "050000" And _
			_PixelGetColor_GetPixel($vDC, $x + 13, $y + 1, $hDll) == "0A0000" And _
			_PixelGetColor_GetPixel($vDC, $x + 11, $y + 2, $hDll) == "B00200" And _
			_PixelGetColor_GetPixel($vDC, $x + 12, $y + 2, $hDll) == "AA0300" And _
			_PixelGetColor_GetPixel($vDC, $x + 13, $y + 2, $hDll) == "9D0200" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfDemonLegs
Func CheckIfDragonScaleLegs($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 11, $y + 1, $hDll) == "095F0C" And _
			_PixelGetColor_GetPixel($vDC, $x + 12, $y + 1, $hDll) == "050505" And _
			_PixelGetColor_GetPixel($vDC, $x + 13, $y + 1, $hDll) == "3ED053" And _
			_PixelGetColor_GetPixel($vDC, $x + 11, $y + 2, $hDll) == "095F0C" And _
			_PixelGetColor_GetPixel($vDC, $x + 12, $y + 2, $hDll) == "050505" And _
			_PixelGetColor_GetPixel($vDC, $x + 13, $y + 2, $hDll) == "37D251" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfDragonScaleLegs
Func CheckIfIcyCulottes($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "145E89" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "145E89" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "153575" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "3E86B0" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "145E89" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "153575" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfIcyCulottes
Func CheckIfOrnateLegs($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 10, $y + 3, $hDll) == "6A5841" And _
			_PixelGetColor_GetPixel($vDC, $x + 11, $y + 3, $hDll) == "2f2118" And _
			_PixelGetColor_GetPixel($vDC, $x + 12, $y + 3, $hDll) == "100B09" And _
			_PixelGetColor_GetPixel($vDC, $x + 10, $y + 4, $hDll) == "100B09" And _
			_PixelGetColor_GetPixel($vDC, $x + 11, $y + 4, $hDll) == "988667" And _
			_PixelGetColor_GetPixel($vDC, $x + 12, $y + 4, $hDll) == "C6B394" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfOrnateLegs
Func CheckIfAmazonShield($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "180002" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "230100" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "020100" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "A60000" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "EC2100" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "9F0000" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfAmazonShield
Func CheckIfBlessedShield($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "C6A500" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "FF00FF" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "B50063" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "C6A500" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "C69C00" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "9C6B00" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfBlessedShield
Func CheckIfBladeOfCarvingMayhemRemedy($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "9A9A9A" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "727272" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "999999" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "1E1E1E" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "9A9A9A" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "727272" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfBladeOfCarvingMayhemRemedy
Func CheckIfDemonwingAxe($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "243235" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "5A7174" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "607B80" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "2E4043" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "0C1111" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfDemonwingAxe
Func CheckIfGreatAxe($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 13, $y + 14, $hDll) == "6D6455" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "5F5856" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "5F5856" And _
			_PixelGetColor_GetPixel($vDC, $x + 13, $y + 15, $hDll) == "686361" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "4A4742" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "020300" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfGreatAxe
Func CheckIfHammerOfProphecy($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "3DB5B5" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "36C4C4" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "279696" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "349AAE" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "3399AC" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "2B9FB3" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfHammerOfProphecy
Func CheckIfHavocBlade($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "45574C" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "CBCBDE" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "FFFFFF" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "9696A4" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "4D5E55" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "C5C5D6" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfHavocBlade
Func CheckIfImpalerOfTheIgniter($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 12, $y + 14, $hDll) == "8E2032" And _
			_PixelGetColor_GetPixel($vDC, $x + 13, $y + 14, $hDll) == "66121C" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "010101" And _
			_PixelGetColor_GetPixel($vDC, $x + 12, $y + 15, $hDll) == "922033" And _
			_PixelGetColor_GetPixel($vDC, $x + 13, $y + 15, $hDll) == "68121C" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "66121C" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfImpalerOfTheIgniter
Func CheckIfMagicLongsword($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "BBC2E7" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "393F67" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "333647" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "8D97D0" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "BBC2E7" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "656B8D" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfMagicLongsword
Func CheckIfMythrilAxe($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "0A3352" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "092941" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "C10f8C" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "052A46" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "0D3E62" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "0B2537" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfMythrilAxe
Func CheckIfPharaoSword($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "CBCBCB" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "333333" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "FFDA00" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "333333" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "FFDA00" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "BE6B00" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfPharaoSword
Func CheckIfPlagueBite($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "5D5C54" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "25141D" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "25141D" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "384040" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "384040" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "384040" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfPlagueBite
Func CheckIfRavagersAxe($x, $y)

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $found_item = 0
	;===== Boots of Haste =====
	If _PixelGetColor_GetPixel($vDC, $x + 14, $y + 14, $hDll) == "744A0C" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 14, $hDll) == "7D4F0D" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 14, $hDll) == "7A4E0D" And _
			_PixelGetColor_GetPixel($vDC, $x + 14, $y + 15, $hDll) == "000000" And _
			_PixelGetColor_GetPixel($vDC, $x + 15, $y + 15, $hDll) == "9E6411" And _
			_PixelGetColor_GetPixel($vDC, $x + 16, $y + 15, $hDll) == "A36711" Then
		$found_item = 1
	EndIf

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

	If $found_item == 1 Then
		Return True
	EndIf

	Return False
EndFunc   ;==>CheckIfRavagersAxe




Func DrawCharacterName()


	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $x = 0
	Local $y = 0


	For $x1 = $PLAYER_NAME_RECTANGLE[0] To $PLAYER_NAME_RECTANGLE[2]
		For $y1 = $PLAYER_NAME_RECTANGLE[1] To $PLAYER_NAME_RECTANGLE[3]
			Local $color = _PixelGetColor_GetPixel($vDC, $x1, $y1, $hDll)
			GUICtrlCreateLabel("", 220 + $x, 50 + $y, 1, 1)
			GUICtrlSetBkColor(-1, "0x" & $color)
			FileWrite(@ScriptDir & "\character_name_px_color.txt", @CRLF & $x & ", " & $y & ", " & $color)
			$y = $y + 1
		Next
		$y = 0
		$x = $x + 1
	Next

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)


EndFunc   ;==>DrawCharacterName
Func TibiaWindowExists()
	If WinExists("[CLASS:Qt5QWindowIcon]", "") Then
		Return True
	EndIf
	Return False
EndFunc   ;==>TibiaWindowExists
Func TibiaWindowIsFocused()
	If WinActive("[CLASS:Qt5QWindowIcon]") Then
		Return True
	EndIf
	Return False
EndFunc   ;==>TibiaWindowIsFocused
Func CharacterLogedIn()
	If WinGetTitle("[CLASS:Qt5QWindowIcon]") <> "Tibia" Then
		Return True
	EndIf
	Return False
EndFunc   ;==>CharacterLogedIn
Func ActivateTibiaWindow()
	WinActivate("[CLASS:Qt5QWindowIcon]")
EndFunc   ;==>ActivateTibiaWindow
Func SaveCurrentTibiaWindowPos($log)

	Local $time = TimerInit()

	If TibiaWindowExists() Then
		Local $pos = GetCurrentTibiaWindowPos()

		$BLACK_WINDOW_X = $pos[0]
		$BLACK_WINDOW_Y = $pos[1]
		$BLACK_WINDOW_WIDTH = $pos[2]
		$BLACK_WINDOW_HEIGHT = $pos[3]
	EndIf

	Local $diff = TimerDiff($time)

	If $BLACK_WINDOW_WIDTH <> 0 And $BLACK_WINDOW_HEIGHT <> 0 Then
		If $log == 1 Then
			$hFileOpen = FileOpen($sFilePath, 1)
			FileWrite($hFileOpen, @CRLF & "====== Tibia Client Position & Size ======")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |TibiaClient.found = True")
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |X: " & $BLACK_WINDOW_X)
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Y: " & $BLACK_WINDOW_Y)
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |WIDTH: " & $BLACK_WINDOW_WIDTH)
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |HEIGHT: " & $BLACK_WINDOW_HEIGHT)
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
			FileWrite($hFileOpen, @CRLF)
			FileClose($hFileOpen)
		EndIf
	Else
		If $log == 1 Then
			$hFileOpen = FileOpen($sFilePath, 1)
			FileWrite($hFileOpen, @CRLF & "====== Tibia Client Position & Size ======")
			FileWrite($hFileOpen, @CRLF & "|TibiaClient.found = False")
			FileWrite($hFileOpen, @CRLF & "|X: " & $BLACK_WINDOW_X)
			FileWrite($hFileOpen, @CRLF & "|Y: " & $BLACK_WINDOW_Y)
			FileWrite($hFileOpen, @CRLF & "|WIDTH: " & $BLACK_WINDOW_WIDTH)
			FileWrite($hFileOpen, @CRLF & "|HEIGHT: " & $BLACK_WINDOW_HEIGHT)
			FileWrite($hFileOpen, @CRLF & _NowTime() & " |Process Time: " & $diff)
			FileWrite($hFileOpen, @CRLF)
			FileClose($hFileOpen)
		EndIf
	EndIf


EndFunc   ;==>SaveCurrentTibiaWindowPos
Func GetCurrentTibiaWindowPos()

	If TibiaWindowExists() Then
		Local $pos = WinGetPos("[CLASS:Qt5QWindowIcon]")

		Local $tempPos[4] = [$pos[0], $pos[1], $pos[2], $pos[3]]
		Return $tempPos
	EndIf

EndFunc   ;==>GetCurrentTibiaWindowPos


Func CloseCurrentChatChannel()
	If Not TibiaWindowIsFocused() Then
		ActivateTibiaWindow()
		Sleep(100)
		Send("^{e}")
	Else
		Sleep(100)
		Send("^{e}")
	EndIf
EndFunc   ;==>CloseCurrentChatChannel

Func AntiIdle()

	If $AntiIdle == 1 Then

		If TibiaWindowExists() And TibiaWindowIsFocused() And CharacterLogedIn() Then
			GetBlackIdleTime()
		EndIf

		ConsoleWrite(@CRLF & "IDLE TIME: " & TimerDiff($BLACK_IDLE_TIME))

		If TimerDiff($BLACK_IDLE_TIME) >= $ANTI_IDLE_TIME Then
			$BLACK_IDLE_TIME = TimerInit()
			If TibiaWindowExists() And CharacterLogedIn() Then
				ActivateTibiaWindow()
				CharacterLook("North")
				Sleep(500)
				CharacterLook("East")
				Sleep(500)
				CharacterLook("South")
				Sleep(500)
				CharacterLook("West")
				Sleep(500)
				CharacterLook("North")
				Sleep(500)
			EndIf
		EndIf
	EndIf
EndFunc   ;==>AntiIdle
Func GetBlackIdleTime()
	If _IsArrowKeyPressed() == 1 Then
		$BLACK_IDLE_TIME = TimerInit()
	EndIf
EndFunc   ;==>GetBlackIdleTime
Func StopBlack()
	GUICtrlSetState($chkbox_Start, $GUI_UNCHECKED)
	$BLACK_STATUS = 0
EndFunc   ;==>StopBlack
Func StartBot()
	GUICtrlSetState($chkbox_Start, $GUI_CHECKED)
	$BLACK_STATUS = 1
EndFunc   ;==>StartBot


Func OnClick_chkbox_Start()

	Local $tempStatus = GUICtrlRead($status)

	If Not $CONTAINERS_READY Then
		_GUICtrlEdit_ShowBalloonTip($input_Scan_info, "", "Please scan first your containers!", $TTI_INFO)
		GUICtrlSetState($chkbox_Start, $GUI_UNCHECKED)
	Else
		If GUICtrlRead($chkbox_Start) = $GUI_UNCHECKED Then
			$BLACK_STATUS = 0
		ElseIf GUICtrlRead($chkbox_Start) = $GUI_CHECKED Then
			$BLACK_STATUS = 1
			ActivateTibiaWindow()
		EndIf
	EndIf
EndFunc   ;==>OnClick_chkbox_Start
Func OnClick_radiobox_south()
	If GUICtrlRead($radiobox_south) = $GUI_UNCHECKED Then
		$BLACK_NORTH = True
		$BLACK_SOUTH = False
	ElseIf GUICtrlRead($radiobox_south) = $GUI_CHECKED Then
		$BLACK_NORTH = False
		$BLACK_SOUTH = True
	EndIf
	ConsoleWrite(@CRLF & "$BLACK_NORTH = " & $BLACK_NORTH)
	ConsoleWrite(@CRLF & "$BLACK_SOUTH = " & $BLACK_SOUTH)
EndFunc   ;==>OnClick_radiobox_south
Func OnClick_radiobox_north()
	If GUICtrlRead($radiobox_north) = $GUI_UNCHECKED Then
		$BLACK_NORTH = False
		$BLACK_SOUTH = True
	ElseIf GUICtrlRead($radiobox_north) = $GUI_CHECKED Then
		$BLACK_NORTH = True
		$BLACK_SOUTH = False
	EndIf
	ConsoleWrite(@CRLF & "$BLACK_NORTH = " & $BLACK_NORTH)
	ConsoleWrite(@CRLF & "$BLACK_SOUTH = " & $BLACK_SOUTH)
EndFunc   ;==>OnClick_radiobox_north
Func OnClick_radiobox_west()
	If GUICtrlRead($radiobox_west) = $GUI_UNCHECKED Then
		$BLACK_EAST = True
		$BLACK_WEST = False
	ElseIf GUICtrlRead($radiobox_west) = $GUI_CHECKED Then
		$BLACK_EAST = False
		$BLACK_WEST = True
	EndIf
	ConsoleWrite(@CRLF & "$BLACK_EAST = " & $BLACK_EAST)
	ConsoleWrite(@CRLF & "$BLACK_WEST = " & $BLACK_WEST)
EndFunc   ;==>OnClick_radiobox_west
Func OnClick_radiobox_east()
	If GUICtrlRead($radiobox_east) = $GUI_UNCHECKED Then
		$BLACK_EAST = False
		$BLACK_WEST = True
	ElseIf GUICtrlRead($radiobox_east) = $GUI_CHECKED Then
		$BLACK_EAST = True
		$BLACK_WEST = False
	EndIf
	ConsoleWrite(@CRLF & "$BLACK_EAST = " & $BLACK_EAST)
	ConsoleWrite(@CRLF & "$BLACK_WEST = " & $BLACK_WEST)
EndFunc   ;==>OnClick_radiobox_east
Func OnClick_chkbox_DWM()
	If GUICtrlRead($chkbox_DWM) = $GUI_UNCHECKED Then
		DllCall("dwmapi.dll", "hwnd", "DwmEnableComposition", "uint", 1)
	ElseIf GUICtrlRead($chkbox_DWM) = $GUI_CHECKED Then
		DllCall("dwmapi.dll", "hwnd", "DwmEnableComposition", "uint", 0)
	EndIf
EndFunc   ;==>OnClick_chkbox_DWM


;=========== TEMP FUNCTIONS
Func SaveColors()

	Local $startScanX = $BLACK_WINDOW_X
	Local $startScanY = $BLACK_WINDOW_Y
	Local $endScanX = $BLACK_WINDOW_X + $BLACK_WINDOW_WIDTH
	Local $endScanY = $BLACK_WINDOW_Y + $BLACK_WINDOW_HEIGHT

	Local $hDll = DllOpen("gdi32.dll")
	Local $vDC = _PixelGetColor_CreateDC($hDll)
	Local $vRegion = _PixelGetColor_CaptureRegion($vDC, 0, 0, @DesktopWidth, @DesktopHeight, $hDll)

	Local $temp = 0
	For $y = 27 To 203
		$color = _PixelGetColor_GetPixel($vDC, 873, $y, $hDll)
		FileWrite(@ScriptDir & "\ActionWindow_RightRectangleColors.txt", @CRLF & "$ArrayColor[" & $temp & "] = " & "0x" & StringLower($color))
		$temp = $temp + 1
	Next

	_PixelGetColor_ReleaseRegion($vRegion)
	_PixelGetColor_ReleaseDC($vDC, $hDll)
	DllClose($hDll)

EndFunc   ;==>SaveColors

Func ColorArray()
	$ArrayColor[0] = 0x424243
	$ArrayColor[1] = 0x4f4f50
	$ArrayColor[2] = 0x444444
	$ArrayColor[3] = 0x49494a
	$ArrayColor[4] = 0x4a4a4b
	$ArrayColor[5] = 0x404041
	$ArrayColor[6] = 0x4c4c4d
	$ArrayColor[7] = 0x444444
	$ArrayColor[8] = 0x4a4a4b
	$ArrayColor[9] = 0x484849
	$ArrayColor[10] = 0x4c4c4d
	$ArrayColor[11] = 0x404041
	$ArrayColor[12] = 0x3f4040
	$ArrayColor[13] = 0x464647
	$ArrayColor[14] = 0x424243
	$ArrayColor[15] = 0x484849
	$ArrayColor[16] = 0x424243
	$ArrayColor[17] = 0x484848
	$ArrayColor[18] = 0x464647
	$ArrayColor[19] = 0x444445
	$ArrayColor[20] = 0x404041
	$ArrayColor[21] = 0x484848
	$ArrayColor[22] = 0x464647
	$ArrayColor[23] = 0x515251
	$ArrayColor[24] = 0x484849
	$ArrayColor[25] = 0x484849
	$ArrayColor[26] = 0x414241
	$ArrayColor[27] = 0x49494a
	$ArrayColor[28] = 0x494a4a
	$ArrayColor[29] = 0x505051
	$ArrayColor[30] = 0x454646
	$ArrayColor[31] = 0x464647
	$ArrayColor[32] = 0x494a49
	$ArrayColor[33] = 0x424243
	$ArrayColor[34] = 0x404040
	$ArrayColor[35] = 0x484848
	$ArrayColor[36] = 0x404040
	$ArrayColor[37] = 0x494a4a
	$ArrayColor[38] = 0x454645
	$ArrayColor[39] = 0x444444
	$ArrayColor[40] = 0x505051
	$ArrayColor[41] = 0x424243
	$ArrayColor[42] = 0x464647
	$ArrayColor[43] = 0x4a4a4b
	$ArrayColor[44] = 0x4c4c4d
	$ArrayColor[45] = 0x404041
	$ArrayColor[46] = 0x494a49
	$ArrayColor[47] = 0x444444
	$ArrayColor[48] = 0x3c3c3c
	$ArrayColor[49] = 0x3e3e3f
	$ArrayColor[50] = 0x4a4a4b
	$ArrayColor[51] = 0x444444
	$ArrayColor[52] = 0x444445
	$ArrayColor[53] = 0x444445
	$ArrayColor[54] = 0x424243
	$ArrayColor[55] = 0x444445
	$ArrayColor[56] = 0x505050
	$ArrayColor[57] = 0x444444
	$ArrayColor[58] = 0x464647
	$ArrayColor[59] = 0x464647
	$ArrayColor[60] = 0x464647
	$ArrayColor[61] = 0x4c4c4c
	$ArrayColor[62] = 0x454646
	$ArrayColor[63] = 0x484848
	$ArrayColor[64] = 0x454646
	$ArrayColor[65] = 0x454646
	$ArrayColor[66] = 0x444444
	$ArrayColor[67] = 0x484849
	$ArrayColor[68] = 0x494a4a
	$ArrayColor[69] = 0x4c4c4d
	$ArrayColor[70] = 0x444445
	$ArrayColor[71] = 0x3e3e3f
	$ArrayColor[72] = 0x444445
	$ArrayColor[73] = 0x3c3b3c
	$ArrayColor[74] = 0x454646
	$ArrayColor[75] = 0x444445
	$ArrayColor[76] = 0x454646
	$ArrayColor[77] = 0x444444
	$ArrayColor[78] = 0x3d3e3e
	$ArrayColor[79] = 0x3f3f40
	$ArrayColor[80] = 0x464648
	$ArrayColor[81] = 0x4c4c4d
	$ArrayColor[82] = 0x454646
	$ArrayColor[83] = 0x444444
	$ArrayColor[84] = 0x4b4c4b
	$ArrayColor[85] = 0x404040
	$ArrayColor[86] = 0x494a4a
	$ArrayColor[87] = 0x4a4a4b
	$ArrayColor[88] = 0x494a4a
	$ArrayColor[89] = 0x4c4c4d
	$ArrayColor[90] = 0x444444
	$ArrayColor[91] = 0x494a49
	$ArrayColor[92] = 0x484849
	$ArrayColor[93] = 0x444445
	$ArrayColor[94] = 0x494a4a
	$ArrayColor[95] = 0x49494a
	$ArrayColor[96] = 0x424243
	$ArrayColor[97] = 0x4f4f50
	$ArrayColor[98] = 0x444444
	$ArrayColor[99] = 0x49494a
	$ArrayColor[100] = 0x4a4a4b
	$ArrayColor[101] = 0x404041
	$ArrayColor[102] = 0x4c4c4d
	$ArrayColor[103] = 0x444444
	$ArrayColor[104] = 0x4a4a4b
	$ArrayColor[105] = 0x484849
	$ArrayColor[106] = 0x4c4c4d
	$ArrayColor[107] = 0x404041
	$ArrayColor[108] = 0x3f4040
	$ArrayColor[109] = 0x464647
	$ArrayColor[110] = 0x424243
	$ArrayColor[111] = 0x484849
	$ArrayColor[112] = 0x424243
	$ArrayColor[113] = 0x484848
	$ArrayColor[114] = 0x464647
	$ArrayColor[115] = 0x444445
	$ArrayColor[116] = 0x404041
	$ArrayColor[117] = 0x484848
	$ArrayColor[118] = 0x464647
	$ArrayColor[119] = 0x515251
	$ArrayColor[120] = 0x484849
	$ArrayColor[121] = 0x484849
	$ArrayColor[122] = 0x414241
	$ArrayColor[123] = 0x49494a
	$ArrayColor[124] = 0x494a4a
	$ArrayColor[125] = 0x505051
	$ArrayColor[126] = 0x454646
	$ArrayColor[127] = 0x464647
	$ArrayColor[128] = 0x494a49
	$ArrayColor[129] = 0x424243
	$ArrayColor[130] = 0x404040
	$ArrayColor[131] = 0x484848
	$ArrayColor[132] = 0x404040
	$ArrayColor[133] = 0x494a4a
	$ArrayColor[134] = 0x454645
	$ArrayColor[135] = 0x444444
	$ArrayColor[136] = 0x505051
	$ArrayColor[137] = 0x424243
	$ArrayColor[138] = 0x464647
	$ArrayColor[139] = 0x4a4a4b
	$ArrayColor[140] = 0x4c4c4d
	$ArrayColor[141] = 0x404041
	$ArrayColor[142] = 0x494a49
	$ArrayColor[143] = 0x444444
	$ArrayColor[144] = 0x3c3c3c
	$ArrayColor[145] = 0x3e3e3f
	$ArrayColor[146] = 0x4a4a4b
	$ArrayColor[147] = 0x444444
	$ArrayColor[148] = 0x444445
	$ArrayColor[149] = 0x444445
	$ArrayColor[150] = 0x424243
	$ArrayColor[151] = 0x444445
	$ArrayColor[152] = 0x505050
	$ArrayColor[153] = 0x444444
	$ArrayColor[154] = 0x464647
	$ArrayColor[155] = 0x464647
	$ArrayColor[156] = 0x464647
	$ArrayColor[157] = 0x4c4c4c
	$ArrayColor[158] = 0x454646
	$ArrayColor[159] = 0x484848
	$ArrayColor[160] = 0x454646
	$ArrayColor[161] = 0x454646
	$ArrayColor[162] = 0x444444
	$ArrayColor[163] = 0x484849
	$ArrayColor[164] = 0x494a4a
	$ArrayColor[165] = 0x4c4c4d
	$ArrayColor[166] = 0x444445
	$ArrayColor[167] = 0x3e3e3f
	$ArrayColor[168] = 0x444445
	$ArrayColor[169] = 0x3c3b3c
	$ArrayColor[170] = 0x454646
	$ArrayColor[171] = 0x444445
	$ArrayColor[172] = 0x454646
	$ArrayColor[173] = 0x444444
	$ArrayColor[174] = 0x3d3e3e
	$ArrayColor[175] = 0x3f3f40
	$ArrayColor[176] = 0x464648
EndFunc   ;==>ColorArray




;======= Text Logfile Functions =======
Func ClearLogFile()
	Local $hFileOpen = FileOpen($sFilePath, 2)
	FileWrite($hFileOpen, "")
	FileClose($hFileOpen)

	Local $DateTime = _NowTime()
	Local $Date = _NowDate()

	$hFileOpen = FileOpen($sFilePath, 1)
	FileWrite($hFileOpen, @CRLF & "====== BLACK LOG SESSION ======")
	FileWrite($hFileOpen, @CRLF & "Date: " & $Date)
	FileWrite($hFileOpen, @CRLF & "Time: " & $DateTime)
	FileWrite($hFileOpen, @CRLF)
	FileClose($hFileOpen)
EndFunc   ;==>ClearLogFile




