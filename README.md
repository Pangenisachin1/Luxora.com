import { useState } from 'react'
import { Button } from "/components/ui/button"
import { Card, CardContent, CardDescription, CardHeader, CardTitle } from "/components/ui/card"
import { Avatar, AvatarFallback, AvatarImage } from "/components/ui/avatar"
import { Heart } from "lucide-react"

interface Product {
  id: number
  name: string
  price: number
  image: string
}

const mockProducts: Product[] = [
  { id: 1, name: "Product 1", price: 29.99, image: "https://via.placeholder.com/150" },
  { id: 2, name: "Product 2", price: 49.99, image: "https://via.placeholder.com/150" },
  { id: 3, name: "Product 3", price: 19.99, image: "https://via.placeholder.com/150" },
]

interface CartItem extends Product {
  quantity: number
}

const Luxora = () => {
  const [cart, setCart] = useState<CartItem[]>([])

  const addToCart = (product: Product) => {
    const existingItem = cart.find(item => item.id === product.id)
    if (existingItem) {
      setCart(cart.map(item => item.id === product.id ? { ...item, quantity: item.quantity + 1 } : item))
    } else {
      setCart([...cart, { ...product, quantity: 1 }])
    }
  }

  const removeFromCart = (productId: number) => {
    setCart(cart.filter(item => item.id !== productId))
  }

  const calculateTotal = () => {
    return cart.reduce((total, item) => total + item.price * item.quantity, 0)
  }

  return (
    <div className="bg-white min-h-screen flex flex-col items-center p-4">
      <h1 className="text-4xl font-bold mb-8">LUXORA</h1>
      <div className="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4 mb-8">
        {mockProducts.map(product => (
          <Card key={product.id} className="w-full max-w-sm">
            <CardHeader>
              <CardTitle>{product.name}</CardTitle>
              <CardDescription>${product.price.toFixed(2)}</CardDescription>
            </CardHeader>
            <CardContent className="flex flex-col items-center justify-center">
              <Avatar className="w-24 h-24 mb-4">
                <AvatarImage src={product.image} />
                <AvatarFallback>{product.name[0]}</AvatarFallback>
              </Avatar>
              <Button onClick={() => addToCart(product)} className="w-full">
                Add to Cart
              </Button>
            </CardContent>
          </Card>
        ))}
      </div>
      <div className="w-full max-w-2xl">
        <Card>
          <CardHeader>
            <CardTitle>Shopping Cart</CardTitle>
          </CardHeader>
          <CardContent>
            {cart.length === 0 ? (
              <p className="text-center">Your cart is empty</p>
            ) : (
              <div className="space-y-4">
                {cart.map(item => (
                  <div key={item.id} className="flex items-center justify-between">
                    <div className="flex items-center">
                      <Avatar className="w-12 h-12 mr-4">
                        <AvatarImage src={item.image} />
                        <AvatarFallback>{item.name[0]}</AvatarFallback>
                      </Avatar>
                      <div>
                        <p className="font-bold">{item.name}</p>
                        <p className="text-gray-500">${item.price.toFixed(2)} x {item.quantity}</p>
                      </div>
                    </div>
                    <Button variant="destructive" onClick={() => removeFromCart(item.id)}>
                      Remove
                    </Button>
                  </div>
                ))}
                <div className="mt-4 flex justify-between">
                  <p className="font-bold">Total:</p>
                  <p className="font-bold">${calculateTotal().toFixed(2)}</p>
                </div>
              </div>
            )}
          </CardContent>
        </Card>
      </div>
    </div>
  )
}

export default Luxora
