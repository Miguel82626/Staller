local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "RK 100%",
   Icon = nil, -- ou assetId válido em string
   LoadingTitle = "RK ACCOUNTS",
   LoadingSubtitle = "by RK_ACCOUNTS",
   Theme = "Default",
   ToggleUIKeybind = "K",

   DisableRayfieldPrompts = false,
   DisableBuildWarnings = false,

   ConfigurationSaving = {
      Enabled = true,
      FolderName = nil,
      FileName = "RK_100%"
   },

   Discord = {
      Enabled = false,
      Invite = "JF2F2RANud",
      RememberJoins = true
   },

   KeySystem = true,
   KeySettings = {
      Title = "RK_100% Keys",
      Subtitle = "Key System",
      Note = "Para conseguir a key, entre no discord da Aura",
      FileName = "Key",
      SaveKey = true,
      GrabKeyFromSite = false,
      Key = {"777", "3265", "RK_777"}
   }
})

local RK_100% = Window:CreateTab("Aura Hub", 4483362458)
local Farm = Window:CreateTab("Farm", 4483362458)

local SectionMain = AuraHub:CreateSection("Funções Principais")

local function MatarJogador()
   local player = game.Players.LocalPlayer
   if player and player.Character and player.Character:FindFirstChild("Humanoid") then
      player.Character.Humanoid.Health = 0
   end
end

local ButtonKill = Farm:CreateButton({
   Name = "Matar Jogador",
   Callback = MatarJogador
})

local ToggleFlash = RK:CreateToggle({
   Name = "Flash",
   CurrentValue = false,
   Callback = function(Value)
      local player = game.Players.LocalPlayer
      if player and player.Character and player.Character:FindFirstChild("Humanoid") then
         if Value then
            player.Character.Humanoid.WalkSpeed = 50
         else
            player.Character.Humanoid.WalkSpeed = 16
         end
      end
   end
})

local SliderJump = Farm:CreateSlider({
   Name = "Pulo (JumpPower)",
   Range = {0, 100},
   Increment = 1,
   Suffix = "", -- sem '%'
   CurrentValue = 50,
   Callback = function(Value)
      local player = game.Players.LocalPlayer
      if player and player.Character and player.Character:FindFirstChild("Humanoid") then
         player.Character.Humanoid.JumpPower = Value
      end
   end
})

local SectionNPC = Farm:CreateSection("NPCs")

local InputNPC = Farm:CreateInput({
   Name = "Nome do NPC",
   PlaceholderText = "Digite um NPC...",
   RemoveTextAfterFocusLost = false,
   Callback = function(Text)
      -- aqui você pode adicionar ação para o texto digitado
   end
})

local DropdownNPC = Farm:CreateDropdown({
   Name = "Selecionar NPC",
   Options = {"Npc 1", "Npc 2", "Npc 3"},
   CurrentOption = "Npc 1",
   Callback = function(Option)
      -- ação para opção selecionada
   end
})

local SectionExtra = Farm:CreateSection("Extras")

local KeybindExample = Farm:CreateKeybind({
   Name = "Atalho de Teclado",
   CurrentKeybind = "F",
   HoldToInteract = false,
   Callback = function(Key)
      -- ação ao pressionar a tecla
   end
})

local ParagraphCreator = Farm:CreateParagraph({
   Title = "Criador",
   Content = "RK ACCOUNTS"
})

Rayfield:LoadConfiguration()
