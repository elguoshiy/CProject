Main.lua

function love.load()

  anim8 = require('anim8')

mainFont = love.graphics.newFont(35)

sprites = {}
sprites.virus = love.graphics.newImage("Sprites/Virus.png")
sprites.VirusBot = love.graphics.newImage("Sprites/VirusBot.png")
sprites.HCells = love.graphics.newImage("Sprites/HCells.png")
sprites.StartButton = love.graphics.newImage("Sprites/StartButton.png")
sprites.GStartButton = love.graphics.newImage("Sprites/GStartButton.png")
sprites.PauseButton = love.graphics.newImage("Sprites/PauseButton.png")
sprites.SelPauseButton = love.graphics.newImage("Sprites/SelPauseButton.png")
sprites.ExitButton = love.graphics.newImage("Sprites/ExitButton.png")
sprites.GExitButton = love.graphics.newImage("Sprites/GExitButton2.png")
sprites.ResumeButton = love.graphics.newImage("Sprites/ResumeButton.png")
sprites.GResumeButton = love.graphics.newImage("Sprites/GResumeButton.png")
sprites.EvolveButton = love.graphics.newImage("Sprites/EvolveButton.png")
sprites.GEvolveButton = love.graphics.newImage("Sprites/GEvolveButton.png")
sprites.BackButton = love.graphics.newImage("Sprites/BackButton.png")
sprites.GBackButton = love.graphics.newImage("Sprites/GBackButton.png")
sprites.BuyButton = love.graphics.newImage("Sprites/BuyButton.png")
sprites.GBuyButton = love.graphics.newImage("Sprites/GBuyButton.png")
sprites.BoughtButton = love.graphics.newImage("Sprites/BoughtButton.png")


sprites.SkillTree = love.graphics.newImage("Sprites/SkillTree.png")
sprites.GGFastButton = love.graphics.newImage("Sprites/GGFastButton.png")
sprites.GGGFastButton = love.graphics.newImage("Sprites/GGGFastButton.png")
sprites.DoubleSplitButton = love.graphics.newImage("Sprites/DoubleSplitButton.png")
sprites.GDoubleSplitButton = love.graphics.newImage("Sprites/GDoubleSplitButton.png")
sprites.HiddenButton = love.graphics.newImage("Sprites/HiddenButton.png")
sprites.GHiddenButton = love.graphics.newImage("Sprites/GHiddenButton.png")
sprites.ProtectionShieldButton = love.graphics.newImage("Sprites/ProtectionShieldButton.png")
sprites.GProtectionShieldButton = love.graphics.newImage("Sprites/GProtectionShieldButton.png")
sprites.LockedButton = love.graphics.newImage("Sprites/LockedButton.png")

sprites.ConfirmationDouble = love.graphics.newImage("Sprites/ConfirmationDouble.png")
sprites.ConfirmationGGFast = love.graphics.newImage("Sprites/ConfirmationGGFast.png")
sprites.ConfirmationHidden = love.graphics.newImage("Sprites/ConfirmationHidden.png")
sprites.ConfirmationShield = love.graphics.newImage("Sprites/ConfirmationShield.png")






require('Player')
require('HCells')
require('SkillTree')
require('Buttons')
local grid = anim8.newGrid(187, 159, sprites.SkillTree:getWidth(), sprites.SkillTree:getHeight())

animations = {}
animations.SkillTree = anim8.newAnimation(grid('1-8',1),0.05)

Virwidth = sprites.virus:getWidth()
love.graphics.setBackgroundColor(1,1,1)
gameState = 1
NumLife = 1
VPoints = 0
SelButton = false

end

function love.update(dt)
PlayerUpdate(dt)
ButtonUpdate(dt)
animations.SkillTree:update(dt)
if DistanceBetween(player.x, HCells.x, player.y, HCells.y) < 55 then
HCells.x = math.random(0, love.graphics.getWidth())
HCells.y = math.random(0, love.graphics.getHeight())
PlayerSpawn()
NumLife = NumLife + 1
VPoints = VPoints + 5
end
end

function love.draw()
if gameState == 1 then
  love.graphics.setFont(mainFont)
love.graphics.setColor(1,0,0)
  love.graphics.printf("Infection", 0, 50, love.graphics.getWidth(),"center")

  love.graphics.setColor(1,1,1)
    love.graphics.draw(StartButton.sprite , StartButton.x, StartButton.y, nil, 0.6, 0.6, sprites.StartButton:getWidth()/2, sprites.StartButton:getHeight()/2)
  end

if gameState == 2 then
  love.graphics.setColor(0,0,0)
  love.graphics.printf("Lives: "..NumLife, 300, 0, love.graphics.getWidth(), "center")
  love.graphics.printf("VPoints: "..VPoints, 100, 0, love.graphics.getWidth(), "left")
love.graphics.setColor(1,1,1)
love.graphics.draw(sprites.HCells, HCells.x, HCells.y, nil, nil ,nil, sprites.HCells:getWidth()/2, sprites.HCells:getHeight()/2)
love.graphics.draw(PauseButton.sprite , PauseButton.x, PauseButton.y, nil, 0.25, 0.25, sprites.PauseButton:getWidth()/2, sprites.PauseButton:getHeight()/2)
love.graphics.draw(sprites.virus, player.x, player.y, nil, 0.4, 0.4, sprites.virus:getWidth()/2, sprites.virus:getHeight()/2)

for i, pb in ipairs(playerBots) do
  love.graphics.draw(sprites.VirusBot, pb.x, pb.y, nil, 0.4, 0.4, sprites.virus:getWidth()/2, sprites.virus:getHeight()/2 )
end
end

if gameState == 3 then
  love.graphics.setFont(mainFont)
love.graphics.setColor(1, 0, 0)
love.graphics.printf("Paused", 300, 25, 200, "center")
love.graphics.setColor(1,1,1)
love.graphics.draw(ExitButton.sprite, ExitButton.x, ExitButton.y, nil, 0.6, 0.6, ExitButton.sprite:getWidth()/2, ExitButton.sprite:getHeight()/2)
love.graphics.draw(ResumeButton.sprite, ResumeButton.x, ResumeButton.y,nil, 0.95, 0.95, ResumeButton.sprite:getWidth()/2, ResumeButton.sprite:getHeight()/2)
love.graphics.draw(EvolveButton.sprite, EvolveButton.x, EvolveButton.y, nil, 0.5, 0.5, EvolveButton.sprite:getWidth()/2, EvolveButton.sprite:getHeight()/2)
end
if gameState == 4 then
  animations.SkillTree:draw(sprites.SkillTree, SkillTree.x, SkillTree.y,nil,2,2)
  love.graphics.draw(BackButton.sprite, BackButton.x, BackButton.y,nil,0.3,0.3, BackButton.sprite:getWidth()/2,BackButton.sprite:getHeight()/2)
  love.graphics.draw(GGFastButton.sprite, GGFastButton.x, GGFastButton.y,nil,0.3,0.3, GGFastButton.sprite:getWidth()/2,GGFastButton.sprite:getHeight()/2)
  love.graphics.draw(ProtectionShieldButton.sprite, ProtectionShieldButton.x, ProtectionShieldButton.y,nil,0.325,0.325, ProtectionShieldButton.sprite:getWidth()/2,ProtectionShieldButton.sprite:getHeight()/2)
  love.graphics.draw(DoubleSplitButton.sprite, DoubleSplitButton.x, DoubleSplitButton.y,nil,0.34,0.34, DoubleSplitButton.sprite:getWidth()/2,DoubleSplitButton.sprite:getHeight()/2)
  love.graphics.draw(HiddenButton.sprite, HiddenButton.x, HiddenButton.y,nil,0.34,0.34, HiddenButton.sprite:getWidth()/2,HiddenButton.sprite:getHeight()/2)
  love.graphics.draw(LockedButtonBL2.sprite, LockedButtonBL2.x, LockedButtonBL2.y, nil, 0.18, 0.18, LockedButtonBL2.sprite:getWidth()/2, LockedButtonBL2.sprite:getHeight()/2)
  love.graphics.draw(LockedButtonBR2.sprite, LockedButtonBR2.x, LockedButtonBR2.y, nil, 0.18, 0.18, LockedButtonBR2.sprite:getWidth()/2, LockedButtonBR2.sprite:getHeight()/2)
  love.graphics.draw(LockedButtonM.sprite, LockedButtonM.x, LockedButtonM.y, nil, 0.18, 0.18, LockedButtonM.sprite:getWidth()/2, LockedButtonM.sprite:getHeight()/2)
  love.graphics.draw(LockedButtonTL.sprite, LockedButtonTL.x, LockedButtonTL.y, nil, 0.18, 0.18, LockedButtonTL.sprite:getWidth()/2, LockedButtonTL.sprite:getHeight()/2)
  love.graphics.draw(LockedButtonTR.sprite, LockedButtonTR.x, LockedButtonTR.y, nil, 0.18, 0.18, LockedButtonTR.sprite:getWidth()/2, LockedButtonTR.sprite:getHeight()/2)
  love.graphics.draw(LockedButtonTM2.sprite, LockedButtonTM2.x, LockedButtonTM2.y, nil, 0.18, 0.18, LockedButtonTM2.sprite:getWidth()/2, LockedButtonTM2.sprite:getHeight()/2)

if Confirmed.HiddenButton == true  and gameState == 4 then
  love.graphics.draw(sprites.ConfirmationHidden,ConfirmationHidden.x,ConfirmationHidden.y, nil, 0.95,0.95,sprites.ConfirmationHidden:getWidth()/2,sprites.ConfirmationHidden:getHeight()/2)
  love.graphics.draw(sprites.BuyButton,BuyButton.x,BuyButton.y, nil, BuyButton.scale,BuyButton.scale,sprites.BuyButton:getWidth()/2,sprites.BuyButton:getHeight()/2)

end
if Confirmed.GGFastButton == true  and gameState == 4 then
  love.graphics.draw(sprites.ConfirmationGGFast,ConfirmationGGFast.x,ConfirmationGGFast.y, nil, 0.95,0.95,sprites.ConfirmationGGFast:getWidth()/2,sprites.ConfirmationGGFast:getHeight()/2)
  love.graphics.draw(sprites.BuyButton,BuyButton.x,BuyButton.y, nil, BuyButton.scale,BuyButton.scale,sprites.BuyButton:getWidth()/2,sprites.BuyButton:getHeight()/2)
end
if Confirmed.DoubleSplitButton == true  and gameState == 4 then
  love.graphics.draw(sprites.ConfirmationDouble,ConfirmationDouble.x,ConfirmationDouble.y, nil, 0.95,0.95,sprites.ConfirmationDouble:getWidth()/2,sprites.ConfirmationDouble:getHeight()/2)
  love.graphics.draw(sprites.BuyButton,BuyButton.x,BuyButton.y, nil, BuyButton.scale,BuyButton.scale,sprites.BuyButton:getWidth()/2,sprites.BuyButton:getHeight()/2)
end
if Confirmed.ProtectionShieldButton == true  and gameState == 4 then
  love.graphics.draw(sprites.ConfirmationShield,ConfirmationShield.x,ConfirmationShield.y, nil, 0.95,0.95,sprites.ConfirmationShield:getWidth()/2,sprites.ConfirmationShield:getHeight()/2)
  love.graphics.draw(sprites.BuyButton,BuyButton.x,BuyButton.y, nil, BuyButton.scale,BuyButton.scale,sprites.BuyButton:getWidth()/2,sprites.BuyButton:getHeight()/2)
end

end

end

function newAnimation(image, width, height, duration)
    local animation = {}
    animation.spriteSheet = image;
    animation.quads = {};

    for y = 0, image:getHeight() - height, height do
        for x = 0, image:getWidth() - width, width do
            table.insert(animation.quads, love.graphics.newQuad(x, y, width, height, image:getDimensions()))
        end
    end

    animation.duration = duration or 1
    animation.currentTime = 0

    return animation
end

function animUpdate(dt)
    animation.currentTime = animation.currentTime + dt
    if animation.currentTime >= animation.duration then
        animation.currentTime = animation.currentTime - animation.duration
    end
end


function DistanceBetween(x1,x2,y1,y2)
return math.sqrt((x2 - x1)^2 + (y2 - y1)^2)
end


Buttons.lua

Confirmed = {}
Confirmed.GGFastButton = false
Confirmed.HiddenButton = false
Confirmed.DoubleSplitButton = false
Confirmed.ProtectionShieldButton = false

StartButton = {}
StartButton.x = 392
StartButton.y = 220
StartButton.size = 110
StartButton.sprite = sprites.StartButton

BuyButton = {}
BuyButton.x = 620
BuyButton.y = 545
BuyButton.scale = 0.65

PauseButton = {}
PauseButton.x = 35
PauseButton.y = 43
PauseButton.width = 33
PauseButton.height = 33
PauseButton.size = 65
PauseButton.sprite = sprites.PauseButton

ExitButton = {}

ExitButton.x = 385
ExitButton.y = 450
ExitButton.sprite = sprites.ExitButton
ExitButton.size = 102

ResumeButton = {}
ResumeButton.x = 385
ResumeButton.y = 150
ResumeButton.sprite = sprites.ResumeButton
ResumeButton.width = 217 + ResumeButton.x
ResumeButton.height = 70
ResumeButton.size = 100

EvolveButton = {}
EvolveButton.x = 385
EvolveButton.y  = 300
EvolveButton.width = 100
EvolveButton.height = 40
EvolveButton.sprite = sprites.EvolveButton
EvolveButton.size = 105

BackButton = {}
BackButton.x = 700
BackButton.y =25
BackButton.size = 80
BackButton.sprite = sprites.BackButton

ConfirmationHidden = {}
ConfirmationHidden.x = 600
ConfirmationHidden.y = 500

ConfirmationShield = {}
ConfirmationShield.x = 600
ConfirmationShield.y = 500

ConfirmationGGFast = {}
ConfirmationGGFast.x = 600
ConfirmationGGFast.y = 500

ConfirmationDouble = {}
ConfirmationDouble.x = 600
ConfirmationDouble.y = 500

function ButtonUpdate(dt)

  if DistanceBetween(EvolveButton.x, love.mouse.getX(), EvolveButton.y, love.mouse.getY()) < EvolveButton.size then
EvolveButton.sprite = sprites.GEvolveButton
else
  EvolveButton.sprite = sprites.EvolveButton
  end

if DistanceBetween(ResumeButton.x, love.mouse.getX(), ResumeButton.y, love.mouse.getY()) < ResumeButton.size then
ResumeButton.sprite = sprites.GResumeButton
else
  ResumeButton.sprite = sprites.ResumeButton
  end

  if DistanceBetween(ExitButton.x, love.mouse.getX(), ExitButton.y, love.mouse.getY()) < ExitButton.size then
ExitButton.sprite = sprites.GExitButton
else
  ExitButton.sprite = sprites.ExitButton
  end

  if DistanceBetween(StartButton.x, love.mouse.getX(), StartButton.y, love.mouse.getY()) < StartButton.size then
StartButton.sprite = sprites.GStartButton
else
  StartButton.sprite = sprites.StartButton
  end

  if DistanceBetween(PauseButton.x, love.mouse.getX(), PauseButton.y, love.mouse.getY()) < PauseButton.width or DistanceBetween(PauseButton.x, love.mouse.getX(), PauseButton.y, love.mouse.getY()) < PauseButton.height then
  PauseButton.sprite = sprites.SelPauseButton
  else
  PauseButton.sprite = sprites.PauseButton
  end

  if DistanceBetween(BackButton.x, love.mouse.getX(), BackButton.y, love.mouse.getY()) < BackButton.size then
BackButton.sprite = sprites.GBackButton
else
BackButton.sprite = sprites.BackButton
  end


end




function love.mousepressed(x, y, b, isTouch)
if DistanceBetween(StartButton.x, love.mouse.getX(), StartButton.y, love.mouse.getY()) < StartButton.size and gameState == 1  and b == 1 then
gameState = 2

end
if DistanceBetween(PauseButton.x, love.mouse.getX(), PauseButton.y, love.mouse.getY()) < PauseButton.size and gameState == 2 and b == 1 then
gameState = 3
end
if DistanceBetween(ExitButton.x, love.mouse.getX(), ExitButton.y, love.mouse.getY()) < ExitButton.size and gameState == 3 and b == 1 then
  gameState = 1
for i, z in ipairs(playerBots) do
  playerBots[i] = nil
  gameState = 1
  player.x = 60
  player.y = 200
  NumLife = 1

end
end

if DistanceBetween(EvolveButton.x, love.mouse.getX(), EvolveButton.y, love.mouse.getY()) < EvolveButton.size and gameState == 3 then
  gameState = 4
end

if DistanceBetween(ResumeButton.x, love.mouse.getX(), ResumeButton.y, love.mouse.getY()) < ResumeButton.size and gameState == 3 and b == 1 then
gameState = 2
end
if DistanceBetween(BackButton.x, love.mouse.getX(), BackButton.y, love.mouse.getY()) < BackButton.size and gameState == 4  and b == 1 then
gameState = 3
end

if DistanceBetween(GGFastButton.x, love.mouse.getX(), GGFastButton.y, love.mouse.getY()) < GGFastButton.size and b == 1 and gameState == 4 then
GGFastButton.sprite = sprites.GGGFastButton
Confirmed.GGFastButton = true
else
GGFastButton.sprite = sprites.GGFastButton
Confirmed.GGFastButton = false
end

if DistanceBetween(ProtectionShieldButton.x, love.mouse.getX(), ProtectionShieldButton.y, love.mouse.getY()) < ProtectionShieldButton.size and b == 1 and gameState == 4 then
ProtectionShieldButton.sprite = sprites.GProtectionShieldButton
Confirmed.ProtectionShieldButton = true
else
  ProtectionShieldButton.sprite = sprites.ProtectionShieldButton
  Confirmed.ProtectionShieldButton = false
end



if DistanceBetween(DoubleSplitButton.x, love.mouse.getX(), DoubleSplitButton.y, love.mouse.getY()) < DoubleSplitButton.size and b == 1 and gameState == 4 then
DoubleSplitButton.sprite = sprites.GDoubleSplitButton
Confirmed.DoubleSplitButton = true
else
DoubleSplitButton.sprite = sprites.DoubleSplitButton
Confirmed.DoubleSplitButton = false
end

if DistanceBetween(HiddenButton.x, love.mouse.getX(), HiddenButton.y, love.mouse.getY()) < HiddenButton.size and b == 1 and gameState == 4 then
HiddenButton.sprite = sprites.GHiddenButton
Confirmed.HiddenButton = true
else
HiddenButton.sprite = sprites.HiddenButton
Confirmed.HiddenButton = false
end
end


SkillTree.lua
SkillTree = {}
SkillTree.x = 35
SkillTree.y = 50
SkillTree.sprite = sprites.SkillTree

GGFastButton = {}
GGFastButton.sprite = sprites.GGFastButton
GGFastButton.x = 45
GGFastButton.y = 375
GGFastButton.size = 35

ProtectionShieldButton = {}
ProtectionShieldButton.sprite = sprites.ProtectionShieldButton
ProtectionShieldButton.x = 142
ProtectionShieldButton.y = 375
ProtectionShieldButton.size = 35

DoubleSplitButton = {}
DoubleSplitButton.sprite = sprites.DoubleSplitButton
DoubleSplitButton.x = 300
DoubleSplitButton.y = 375
DoubleSplitButton.size = 35

HiddenButton = {}
HiddenButton.sprite = sprites.HiddenButton
HiddenButton.x = 400
HiddenButton.y = 375
HiddenButton.size = 35

-- LockedButton(Top or Bottom & left or Right) such as LockedButtonBL, LockedButtonBR, etc.

LockedButtonTL = {}
LockedButtonTL.x = 100
LockedButtonTL.y = 110
LockedButtonTL.sprite = sprites.LockedButton

LockedButtonM = {}
LockedButtonM.x = 225
LockedButtonM.y = 210
LockedButtonM.sprite = sprites.LockedButton

LockedButtonTM2 = {}
LockedButtonTM2.x = 228
LockedButtonTM2.y = 55
LockedButtonTM2.sprite = sprites.LockedButton

LockedButtonTR = {}
LockedButtonTR.x = 329
LockedButtonTR.y = 110
LockedButtonTR.sprite = sprites.LockedButton

LockedButtonBL2 = {}
LockedButtonBL2.x = 95
LockedButtonBL2.y = 275
LockedButtonBL2.sprite = sprites.LockedButton

LockedButtonBR2 = {}
LockedButtonBR2.x = 350
LockedButtonBR2.y = 275
LockedButtonBR2.sprite = sprites.LockedButton


Player.lua
playerBots = {}
player = {}
player.x = 60
player.y = 200
player.xvel = 0
player.yvel = 0
player.w = 55
player.h = 55
player.speed = 400
player.size = 55

function PlayerUpdate(dt)
  if gameState == 2 then
  if love.keyboard.isDown('w') and player.y > 0 + player.size then
player.y = player.y  - player.speed * dt
end

if love.keyboard.isDown('s') and player.y < love.graphics.getHeight() - player.size  then
player.y = player.y  + player.speed * dt
end

if love.keyboard.isDown('a') and player.x > 0 + player.size then
player.x = player.x  - player.speed * dt
end

if love.keyboard.isDown('d') and player.x < love.graphics.getWidth() - player.size  then
player.x = player.x  + player.speed * dt
end

for i, pb in ipairs(playerBots) do
  pb.x = pb.x + math.cos(player_playerBot_angle(pb)) * pb.speed * dt
  pb.y = pb.y + math.sin(player_playerBot_angle(pb)) * pb.speed * dt
end

for i, pb in ipairs(playerBots) do
if DistanceBetween(player.x, pb.x, player.y, pb.y) < 60 then
  pb.speed = 0
else
  pb.speed = 250
end
end

for i, pb in ipairs(playerBots) do
  for j, pb2 in ipairs(playerBots) do
    if DistanceBetween(pb.x,pb2.x,pb.y,pb2.y) < 55 then
      pb.x = pb.x  - pb.speed * dt * -1
      pb.x = pb.x  + pb.speed * dt * -1
      pb.y = pb.y  + pb.speed * dt * -1
      pb.y = pb.y  - pb.speed * dt * -1

    end
  end
  end
end
end



function player_playerBot_angle(playerBot)
return math.atan2(player.y - playerBot.y, player.x - playerBot.x)
end

function DistanceBetween(x1,x2,y1,y2)
return math.sqrt((x2 - x1)^2 + (y2 - y1)^2)
end




function PlayerSpawn()
playerBot = {}
playerBot.x = player.x - 10
playerBot.y = player.y - 10
playerBot.direction = 1
playerBot.xvel = 0
playerBot.yvel = 0
playerBot.w = 55
playerBot.h = 55
playerBot.speed = 250
playerBot.size = player.size

table.insert(playerBots, playerBot)
end


HCells.lua
HCells = {}
HCells.size = 60
HCells.x = math.random(0, love.graphics.getWidth()) - HCells.size
HCells.y = math.random(0, love.graphics.getHeight()) - HCells.size
