//Skeleton Program code for the  AQA A Level Paper 1 Summer 2019 examination
//this code should be used in conjunction with the Preliminary Material
//written by the AQA Programmer Team
//developed in the Visual Studio Community Edition programming environmment

using System;
using System.Collections;
using System.IO;
using System.Collections.Generic;

namespace TextAdventures_JF001
{
    class Program
    {
        const int Inventory = 1001;
        const int MinimumIDForItem = 2001;
        const int IDDifferenceForObjectInTwoLocations = 10000;
        static Random Rnd = new Random();

        struct Place
        {
            public string Description;
            public int id, North, East, South, West, Up, Down;
        }

        struct Character
        {
            public string Name, Description;
            public int ID, CurrentLocation;
        }

        struct Item
        {
            public int ID, Location;
            public string Description, Status, Name, Commands, Results;
        }

        private static string GetInstruction()
        {
            string instruction;
            Console.Write("\n> ");
            instruction = Console.ReadLine().ToLower();
            return instruction;
        }

        private static string ExtractCommand(ref string instruction)
        {
            string command = "";
            if (!instruction.Contains(" "))
            {
                return instruction;
            }
            while (instruction.Length > 0 && instruction[0] != ' ')
            {
                command += instruction[0];
                instruction = instruction.Remove(0, 1);
            }
            while (instruction.Length > 0 && instruction[0] == ' ')
            {
                instruction = instruction.Remove(0, 1);
            }
            return command;
        }

        private static bool Go(List<Character> characters, string direction, Place currentPlace)
        {
            bool moved = true;
            Character you = characters[0];// end p.1
            switch (direction)
            {
                case "north":
                    if (currentPlace.North == 0)
                    {
                        moved = false;
                    }
                    else
                    {
                        you.CurrentLocation = currentPlace.North;
                    }
                    break;

                case "east":
                    if (currentPlace.East == 0)
                    {
                        moved = false;
                    }
                    else
                    {
                        you.CurrentLocation = currentPlace.East;
                    }
                    break;
                case "south":
                    if (currentPlace.South == 0)
                    {
                        moved = false;
                    }
                    else
                    {
                        you.CurrentLocation = currentPlace.South;
                    }
                    break;
                case "west":
                    if (currentPlace.West == 0)
                    {
                        moved = false;
                    }
                    else
                    {
                        you.CurrentLocation = currentPlace.West;
                    }
                    break;
                case "up":
                    if (currentPlace.Up == 0)
                    {
                        moved = false;
                    }
                    else
                    {
                        you.CurrentLocation = currentPlace.Up;
                    }
                    break;
                case "down":
                    if (currentPlace.Down == 0)
                    {
                        moved = false;
                    }
                    else
                    {
                        you.CurrentLocation = currentPlace.Down;
                    }
                    break;
                default:
                    moved = false;
                    break;
            }
            if (!moved)
            { // end p.2
                Console.WriteLine("You are not able to go in that direction.");
            }
            characters[0] = you;
            return moved;
        }

        private static bool Go(Character you, string direction, Place currentPlace)
        {
            bool moved = true;
            switch (direction)
            {
                case "north":
                    if (currentPlace.North == 0)
                    {
                        moved = false;
                    }
                    else
                    {
                        you.CurrentLocation = currentPlace.North;
                    }
                    break;

                case "east":
                    if (currentPlace.East == 0)
                    {
                        moved = false;
                    }
                    else
                    {
                        you.CurrentLocation = currentPlace.East;
                    }
                    break;
                case "south":
                    if (currentPlace.East == 0)
                    {
                        moved = false;
                    }
                    else
                    {
                        you.CurrentLocation = currentPlace.East;
                    }
                    break;
                case "west":
                    if (currentPlace.West == 0)
                    {
                        moved = false;
                    }
                    else
                    {
                        you.CurrentLocation = currentPlace.West;
                    }
                    break;
                case "up":
                    if (currentPlace.Up == 0)
                    {
                        moved = false;
                    }
                    else
                    {
                        you.CurrentLocation = currentPlace.Up;
                    }
                    break;
                case "down":
                    if (currentPlace.Down == 0)
                    {
                        moved = false;
                    }
                    else
                    { //  end p.3
                        you.CurrentLocation = currentPlace.Down;
                    }
                    break;
                default:
                    moved = false;
                    break;
            }
            if (!moved)
            {
                Console.WriteLine("You are not able to go in that direction.");
            }
            return moved;
        }

        private static void DisplayDoorStatus(string status)
        {
            if (status == "open")
            {
                Console.WriteLine("The door is open.");
            }
            else
            {
                Console.WriteLine("The door is closed.");
            }
        }

        private static void DisplayContentsOfContainerItem(List<Item> items, int containerID)
        {
            Console.Write("It contains: ");
            bool containsItem = false;
            foreach (var thing in items)
            {
                if (thing.Location == containerID)
                {
                    if (containsItem)
                    {
                        Console.Write(",  ");
                    }
                    containsItem = true;
                    Console.Write(thing.Name);
                }
                if (containsItem)
                {
                    Console.WriteLine(".");
                }
                else
                {
                    Console.WriteLine("nothing.");
                }
            }
        }

        private static void Examine(List<Item> items, List<Character> characters, string itemToExamine, int currentLocation)
        {
            int Count = 0;
            if (itemToExamine == "inventory")
            {
                DisplayInventory(items);
            }
            else
            {
                int IndexOfItem = GetIndexOfItem(itemToExamine, -1, items);
                if (IndexOfItem != -1)
                {
                    if (items[IndexOfItem].Name == itemToExamine && (items[IndexOfItem].Location == Inventory || items[IndexOfItem].Location == currentLocation)) // end p.4
                    {
                        Console.WriteLine(items[IndexOfItem].Description);
                        if (items[IndexOfItem].Name.Contains("door"))
                        {
                            DisplayDoorStatus(items[IndexOfItem].Status);
                        }
                        if (items[IndexOfItem].Status.Contains("container"))
                        {
                            DisplayContentsOfContainerItem(items, items[IndexOfItem].ID);
                        }
                        return;
                    }
                }
                while (Count < characters.Count)
                {
                    if (characters[Count].Name == itemToExamine && characters[Count].CurrentLocation == currentLocation)
                    {
                        Console.WriteLine(characters[Count].Description);
                        return;
                    }
                    Count++;
                }
                Console.WriteLine("You cannot find " + itemToExamine + " to look at.");
            }
        }

        private static int GetPositionOfCommand(string commandList, string command)
        {
            int position = 0, count = 0;
            while (count <= commandList.Length - command.Length)
            {
                if (commandList.Substring(count, command.Length) == command)
                {
                    return position;
                }
                else if (commandList[count] == ',')
                {
                    position++;
                }
                count++;
            }
            return position;
        }

        private static string GetResultForCommand(string results, int position)
        {
            int count = 0, currentPosition = 0;
            string ResultForCommand = "";
            while (currentPosition < position && count < results.Length)
            {
                if (results[count] == ';')
                {
                    currentPosition++;
                }
                count++;
            }
            while (count < results.Length)
            {
                if (results[count] == ';')
                {
                    break;
                }
                ResultForCommand += results[count];
                count++;
            }
            return ResultForCommand; // end p.5
        }

        private static void Say(string speech)
        {
            Console.WriteLine();
            Console.WriteLine(speech);
            Console.WriteLine();
        }

        private static void ExtractResultForCommand(ref string subCommand, ref string subCommandParameter, string resultForCommand)
        {
            int Count = 0;
            while (Count < resultForCommand.Length && resultForCommand[Count] != ',')
            {
                subCommand += resultForCommand[Count];
                Count++;
            }
            Count++;
            while (Count < resultForCommand.Length)
            {
                if (resultForCommand[Count] != ',' && resultForCommand[Count] != ';')
                {
                    subCommandParameter += resultForCommand[Count];
                }
                else
                {
                    return;
                }
                Count++;
            }
        }

        private static void ChangeLocationReference(string direction, int newLocationReference, List<Place> places, int indexOfCurrentLocation, bool opposite)
        {
            Place thisPlace = places[indexOfCurrentLocation];
            if (direction == "north" && !opposite || direction == "south" && opposite)
            {
                thisPlace.North = newLocationReference;
            }
            else if (direction == "east" && !opposite || direction == "west" && opposite)
            {
                thisPlace.East = newLocationReference;
            }
            else if (direction == "south" && !opposite || direction == "north" && opposite)
            {
                thisPlace.South = newLocationReference;
            }
            else if (direction == "west" && !opposite || direction == "east" && opposite)
            {
                thisPlace.West = newLocationReference;
            }
            else if (direction == "up" && !opposite || direction == "down" && opposite)
            {
                thisPlace.Up = newLocationReference;
            }
            else if (direction == "down" && !opposite || direction == "up" && opposite)
            {
                thisPlace.Down = newLocationReference;  // end p. 6
            }   // top p7. SkeletonCode2019_10pt.txt
            places[indexOfCurrentLocation] = thisPlace;
        }

        private static int OpenClose(bool open, List<Item> items, List<Place> places, string itemToClose, int currentLocation)
        {
            string command, resultForCommand, direction = "", directionChange = "";
            int count = 0, position, count2;
            bool actionWorked = false;
            if (open)
            {
                command = "open";
            }
            else
            {
                command = "close";
            }
            while (count < items.Count && !actionWorked)
            {
                if (items[count].Name == itemToOpenClose)
                {
                    if (items[count].Location == currentLocation)
                    {
                        if (items[count].Commands.Length >= 4)
                        {
                            if (items[count].Status == command)
                            {
                                if (items[count].Status == command)
                                {
                                    return -2;
                                }
                                else if (items[count].Status == "locked")
                                {
                                    return -3;
                                }
                                position = GetPositionOfCommand(items[count].Commands, command);
                                resultForCommand = GetResultForCommand(items[count].Results, position);
                                ExtractResultForCommand(ref direction, ref directionChange, resultForCommand);
                                ChangeStatusOfItem(items, count, command);
                                count2 = 0;
                                actionWorked = true;
                                while (count2 < places.Count)
                                {
                                    if (places[count2].id == Convert.ToInt32(currentLocation))
                                    {
                                        ChangeLocationReference(direction, Convert.ToInt32(directionChange), places, count2, false);
                                    }
                                    else if (places[count2].id == Convert.ToInt32(directionChange))
                                    {
                                        ChangeLocationReference(direction, currentLocation, places, count2, true);
                                    }
                                    count2++;
                                }
                                int IndexofOtherSideDoor;
                                if (items[count].ID > IDDifferenceForObjectInTwoLocations)
                                {  // penultimate line p.7
                                    IndexofOtherSideDoor = GetIndexOfItem("", items[count].ID - IDDifferenceForObjectInTwoLocations, items);
                                }
                                else
                                {
                                    IndexofOtherSideDoor = GetIndexOfItem("", items[count].ID + IDDifferenceForObjectInTwoLocations, items);
                                }
                                ChangeStatusOfItem(items, IndexofOtherSideDoor, command);
                                count = items.Count + 1;
                            }
                        }
                    }
                }
                count++;
            }
            if (!actionWorked)
            {
                return -1;
            }
            return Convert.ToInt32(directionChange);
        }

        private static int GetIndexOfItem(string itemNameToGet, int itemIDToGet, List<Item> items)
        {
            int count = 0;
            bool stopLoop = false;
            while (!stopLoop && count < items.Count)
            {
                if ((itemIDToGet == -1 && items[count].Name == itemNameToGet) || items[count].ID == itemIDToGet)
                {
                    stopLoop = true;
                }
                else
                {
                    count++;
                }
            }
            if (!stopLoop)
            {
                return -1;
            }
            else
            {
                return count;
            }
        }

        private static void ChangeLocationOfItem(List<Item> items, int indexOfItem, int newLocation)
        {
            Item thisItem = items[indexOfItem];
            thisItem.Location = newLocation;
            items[indexOfItem] = thisItem;
        }

        private static void ChangeStatusOfItem(List<Item> items, int indexOfItem, string newStatus)
        {
            Item thisItem = items[indexOfItem];
            thisItem.Status = newStatus;
            items[indexOfItem] = thisItem;
        }

        private static int GetRandomNumber(int lowerLimitValue, int upperLimitValue)  // end p. 8
        {
            return Rnd.Next(lowerLimitValue, upperLimitValue + 1);
        }

        private static int RollDie(string lower, string upper)
        {
            int lowerLimitValue = 0;
            if (!int.TryParse(lower, out lowerLimitValue))
            {
                while (lowerLimitValue < 1 || lowerLimitValue > 6)
                {
                    Console.Write("Enter minimum: ");
                    int.TryParse(Console.ReadLine(), out lowerLimitValue);
                }
            }
            int upperLimitValue = 0;
            if (!int.TryParse(upper, out upperLimitValue))
            {
                while (upperLimitValue < lowerLimitValue || upperLimitValue > 6)
                {
                    Console.Write("Enter maximum: ");
                    int.TryParse(Console.ReadLine(), out upperLimitValue);
                }
            }
            return GetRandomNumber(lowerLimitValue, upperLimitValue);
        }

        private static void ChangeStatusOfDoor(List<Item> items, int currentLocation, int indexOfItemToLockUnlock, int indexOfOtherSideItemToLockUnlock)
        {
            if (currentLocation == items[indexOfItemToLockUnlock].Location || currentLocation == items[indexOfOtherSideItemToLockUnlock].Location)
            {
                if (items[indexOfItemToLockUnlock].Status == "locked")
                {
                    ChangeStatusOfItem(items, indexOfItemToLockUnlock, "close");
                    ChangeStatusOfItem(items, indexOfOtherSideItemToLockUnlock, "close");
                    Say(items[indexOfItemToLockUnlock].Name + " now unlocked.");
                }
                else if (items[indexOfItemToLockUnlock].Status == "close")
                {
                    ChangeStatusOfItem(items, indexOfItemToLockUnlock, "locked");
                    ChangeStatusOfItem(items, indexOfOtherSideItemToLockUnlock, "locked");
                    Say(items[indexOfItemToLockUnlock].Name + " now locked.");
                }
                else
                {
                    Say(items[indexOfItemToLockUnlock].Name + "is open so can't be locked.");
                }
            }
            else
            {
                Say("Can't use that key in this location.");
            }
        }

        private static void UseItem(List<Item> items, string itemToUse, int currentLocation, ref bool stopGame, List<Place> places)
        {
            int position, indexOfItem;
            string resultForCommand, subCommand = "", subCommandParameter = "";
            indexOfItem = GetIndexOfItem(itemToUse, -1, items);
            if (indexOfItem != -1)
            {

            }
            }
            }
        }
    }
}



































































































































































































































































        private static void DisplayInventory(List<Item> items) //Line 840
        {
            Console.WriteLine();
            Console.WriteLine("You are currently carrying the following items:");
            foreach (var thing in items) //Line 844
            {
                if (thing.Location == Inventory)
                {
                    Console.WriteLine(thing.Name);
                }
            }
            Console.WriteLine(); // line 851
        }
