from Tkinter import *
import ttk
import tkMessageBox

#################################################################################################################
class Product:

    dailyProfit = 0
    totalProducts = 0
    product_list = []
    product_refill = {}

    def __init__(self, name, price, quantity):
        self.price = price
        self.name = name
        self.quantity = quantity
        Product.product_refill[name] = 0
        
    def buyFromStock(self, quantity):
        if self.quantity >= quantity:
            self.quantity -= quantity
            Product.totalProducts += quantity
            Product.product_refill[self.name] += quantity
            Product.dailyProfit += int(quantity) * self.price
            return True
        else:
            return False
    
    def changePrice(self, newPrice):
        self.price = newPrice
#################################################################################################################
def main():

    def createWindow():
        createWindow.i = 0
        
        def updateProducts():
            createWindow.i = 1
            for product in Product.product_list:
                ttk.Label(text=product.name + '\t' + str(product.quantity)).grid(row=createWindow.i, column=1, sticky=(W,E))
                createWindow.i += 1
        
        def updateProfit():
            ttk.Label(text= 'Profit of the day so far:', font = "bold").grid(row=createWindow.i, column=1, sticky=(W,E))
            createWindow.i += 1
            ttk.Label(text='%.2f euros.' % Product.dailyProfit).grid(row= createWindow.i, column=1, sticky=(W,E))
            createWindow.i += 1
            ttk.Label(text='%d items sold.' % Product.totalProducts).grid(row= createWindow.i, column=1, sticky=(W,E))
            
        def buyProducts():
            def purchaseCommand():
                product = findProduct(prodName.get())
                if product.buyFromStock(prodQuantity.get()):
                    tkMessageBox._show('Successful Purchase', "You have successfully purchased %d of %s" % (prodQuantity.get(), product.name))
                    updateProducts()
                    updateProfit()
                    topLevel.destroy()

                else:
                    tkMessageBox._show('Unsuccessful Purchase', "Not enough in stock")
                
            topLevel = Toplevel()
            prodName = StringVar()
            prodQuantity = IntVar()
            createTopLevel(topLevel, 'Product Name', prodName, 'Product Quantity', prodQuantity)
            ttk.Button(topLevel, text='Submit', command=purchaseCommand).pack()
        
        def createMessageBox(windowName, windowMessage):
            tkMessageBox._show(windowName, windowMessage)
            updateProducts()
            updateProfit()
        
        def addProducts():
            def newProduct():
                prod = prodDescription.get().split()
                addProduct(prod[0], prod[1], prod[2])
                createMessageBox('Added Successfully', 'You have successfully added %d units of %s to stock.' % (int(prod[2]), prod[0]))
                topLevel.destroy()
            topLevel = Toplevel()
            prodDescription = StringVar()
            createTopLevel(topLevel, 'Product Description', prodDescription)
            ttk.Button(topLevel, text='Submit', command=newProduct).pack()
        
        def checkProducts():
            def checkPrice():
                def checkCommand():
                    prod = findProduct(prodName.get())
                    tkMessageBox._show('Price is', prod.price)
                    topLevel.destroy()
                topLevel = Toplevel()
                prodName = StringVar()
                createTopLevel(topLevel, 'Product Name', prodName)
                ttk.Button(topLevel, text='Submit', command=checkCommand).pack()
            def checkQuantity():
                def checkCommand():
                    prod = findProduct(prodName.get())
                    tkMessageBox._show('We have:', prod.quantity)
                    topLevel.destroy()
                topLevel = Toplevel()
                prodName = StringVar()
                createTopLevel(topLevel, 'Product Name', prodName)
                ttk.Button(topLevel, text='Submit', command=checkCommand).pack()
                
            def checkStock():
                def checkCommand():
                    products = []
                    for product in Product.product_list:
                        products.append(product.name)
                    tkMessageBox._show('We have in stock:', products)
                    topLevel.destroy()
                topLevel = Toplevel()
                ttk.Label(topLevel, text='Check Stock').pack()
                ttk.Button(topLevel, text='Check Stock', command=checkCommand).pack()
            app = Tk()
            app.title('Choose what to check.')
            
            ttk.Button(app, text='Price', command=checkPrice).pack()
            ttk.Button(app, text='Quantity', command=checkQuantity).pack()
            ttk.Button(app, text='Stock', command=checkStock).pack()
            
            app.mainloop()
#################################################################################################################
        def createTopLevel(*args):
            if len(args) == 5:
                ttk.Label(args[0], text=args[1]).pack()
                ttk.Entry(args[0], textvariable=args[2]).pack()
                ttk.Label(args[0], text=args[3]).pack()
                ttk.Entry(args[0], textvariable=args[4]).pack()
            else:
                ttk.Label(args[0], text=args[1]).pack()
                ttk.Entry(args[0], textvariable=args[2]).pack()
#################################################################################################################
        
        def changePrice():
            def purchaseCommand():
                if findProduct(prodName.get()):
                    product = findProduct(prodName.get())
                else:
                    createMessageBox('Error', "That item does not exist")
                    topLevel.destroy()
                    return
                product.changePrice(float(prodQuantity.get()))
                createMessageBox('New Price', "You have successfully changed the price of %s to %.2f" % (product.name, float(prodQuantity.get())))
                topLevel.destroy()
            
            prodName = StringVar()
            prodQuantity = DoubleVar()
            topLevel = Toplevel()
            createTopLevel(topLevel, 'Product Name', prodName, 'New Price', prodQuantity)
            ttk.Button(topLevel, text='Submit', command=purchaseCommand).pack()
        
        def close():
            summary = []
            for item in sorted(Product.product_refill):
                summary.append(item + ': ' + str(Product.product_refill[item]))
            if tkMessageBox.askokcancel('Need to be refilled:', summary):
                root.destroy()

        root = Tk()
        root.title('Product Management')
        mainframe = ttk.Frame(root, padding=(3,3,12,12))
        mainframe.grid(row=0, column=0, sticky=(N,E,S,W))
        
        ttk.Button(root, text='Add', command=addProducts).grid(row=1, column=0)
        ttk.Button(root, text='Buy', command=buyProducts).grid(row=2, column=0)
        ttk.Button(root, text='Check', command=checkProducts).grid(row=3, column=0)
        ttk.Button(root, text='Change', command=changePrice).grid(row=4, column=0)
        ttk.Button(root, text='Close Shop', command=close).grid(row=5, column=0)
        
        ttk.Label(text='Products Available:', font = "bold").grid(row=0, column=1, sticky=(W,E))
        updateProducts()
        ttk.Label(text= 'Profit of the day so far:', font = "bold").grid(row=createWindow.i, column=1, sticky=(W,E))
        updateProfit()
        root.mainloop()
        
#################################################################################################################
    
    def findProduct(name):
        found = False
        for product in Product.product_list:
                    if product.name == name:
                        prod = product
                        found = True
                        break
        if found:
            return prod
        else:
            return
    
    def newDay():
        rice = Product('Rice', 2.99, 600)
        bread = Product('Bread', 1.99, 300)
        cocacola = Product('CocaCola', 4.99, 800)
        Product.product_list.append(bread)
        Product.product_list.append(rice)
        Product.product_list.append(cocacola)
    
    def addProduct(name, price, quantity):
        product = Product(name, float(price), int(quantity))
        Product.product_list.append(product)
    
    # Commands:
    # Add Product.name Product.price Product.id Product.quantity

    newDay()
    createWindow()
    
#################################################################################################################        
if __name__ == '__main__':
    main()  
