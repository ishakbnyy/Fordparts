import { Link } from "wouter";
import { ShoppingCart, Star, Package } from "lucide-react";
import { Button } from "@/components/ui/button";
import { Card, CardContent } from "@/components/ui/card";
import { Badge } from "@/components/ui/badge";
import { useCart } from "@/lib/cart";
import { useToast } from "@/hooks/use-toast";
import type { Product } from "@shared/schema";

interface ProductCardProps {
  product: Product;
}

export default function ProductCard({ product }: ProductCardProps) {
  const { addItem } = useCart();
  const { toast } = useToast();

  const handleAddToCart = (e: React.MouseEvent) => {
    e.preventDefault();
    addItem(product);
    toast({
      title: "Added to cart",
      description: `${product.name} has been added to your cart.`,
    });
  };

  const formatPrice = (price: string) => {
    const numPrice = parseFloat(price);
    return new Intl.NumberFormat('fr-DZ', {
      style: 'currency',
      currency: 'DZD',
      minimumFractionDigits: 0,
    }).format(numPrice);
  };

  const getStockBadge = () => {
    if (!product.stockQuantity || product.stockQuantity === 0) {
      return <Badge variant="destructive">Out of Stock</Badge>;
    }
    if (product.stockQuantity <= 5) {
      return <Badge variant="secondary" className="bg-yellow-100 text-yellow-800">Low Stock</Badge>;
    }
    return <Badge variant="secondary" className="bg-green-100 text-green-800">In Stock</Badge>;
  };

  const isOutOfStock = !product.stockQuantity || product.stockQuantity === 0;

  return (
    <Card className="product-card group overflow-hidden border border-gray-200 hover:shadow-lg transition-all duration-200">
      <Link href={`/product/${product.id}`}>
        <div className="aspect-square overflow-hidden bg-gray-50">
          {product.imageUrl ? (
            <img 
              src={product.imageUrl} 
              alt={product.name}
              className="w-full h-full object-cover group-hover:scale-105 transition-transform duration-200"
            />
          ) : (
            <div className="w-full h-full flex items-center justify-center">
              <Package className="h-16 w-16 text-gray-300" />
            </div>
          )}
        </div>
      </Link>
      
      <CardContent className="p-4">
        <div className="flex items-start justify-between mb-2">
          {getStockBadge()}
          <div className="flex items-center text-yellow-400">
            {[...Array(5)].map((_, i) => (
              <Star key={i} className={`h-3 w-3 ${i < 4 ? 'fill-current' : ''}`} />
            ))}
          </div>
        </div>

        <Link href={`/product/${product.id}`}>
          <h3 className="font-semibold text-gray-900 mb-2 hover:text-primary transition-colors line-clamp-2">
            {product.name}
          </h3>
        </Link>
        
        {product.brand && (
          <p className="text-sm text-gray-600 mb-1">Brand: {product.brand}</p>
        )}
        
        <p className="text-sm text-gray-600 mb-4 line-clamp-2">
          {product.description}
        </p>
        
        <div className="flex items-center justify-between">
          <span className="text-xl font-bold text-primary">
            {formatPrice(product.price)}
          </span>
          <Button 
            size="sm" 
            onClick={handleAddToCart}
            disabled={isOutOfStock}
            className="hover:bg-primary/90"
          >
            <ShoppingCart className="h-4 w-4" />
          </Button>
        </div>
      </CardContent>
    </Card>
  );
}